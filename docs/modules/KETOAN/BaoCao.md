# Báo cáo kế toán

## Feature

Feature này tổng hợp báo cáo doanh thu, chi phí, lợi nhuận, công nợ và view chi tiết.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Báo cáo tháng: `Default.aspx?mod=ketoan!baocao_loinhuanthang`
- Báo cáo năm: `Default.aspx?mod=ketoan!baocao_loinhuannam`
- Báo cáo lợi nhuận theo khoảng ngày: `Default.aspx?mod=ketoan!LOINHUAN`
- Báo cáo công nợ: `Default.aspx?mod=ketoan!baocao_congno`
- View chi tiết mở qua `Frame-ajax.aspx?mod=ketoan!View!...`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/KETOAN/Baocao_loinhuanthang.ascx.cs` | Tổng hợp thu/chi tháng, doanh thu theo loại và doanh thu khóa học theo hard-coded course id. |
| `MODULE/KETOAN/Baocao_loinhuannam.ascx.cs` | Tổng hợp thu/chi theo 12 tháng. |
| `MODULE/KETOAN/LOINHUAN.ascx.cs` | Báo cáo theo khoảng ngày, có tổng công nợ cần thu/cần chi. |
| `MODULE/KETOAN/Baocao_congno.ascx.cs` | Danh sách công nợ theo filter. |
| `MODULE/KETOAN/Baocao_chitiet.ascx.cs` | Báo cáo chi tiết theo năm từ `ThuChi`. |
| `MODULE/KETOAN/Baocao_doanhthu.ascx.cs` | Báo cáo doanh thu. |
| `MODULE/KETOAN/View/View_doanhthu.ascx.cs` | Popup chi tiết thu/chi theo tháng. |
| `MODULE/KETOAN/View/View_phieuthu.ascx.cs` | Popup chi tiết phiếu thu theo `loai`. |
| `MODULE/KETOAN/View/View_thungay.ascx.cs` | Thu trong ngày. |
| `MODULE/KETOAN/View/View_chingay.ascx.cs` | Chi trong ngày. |
| `MODULE/KETOAN/View/View_congno.ascx.cs` | Chi tiết công nợ còn lại. |

## Logic báo cáo tháng

`Baocao_loinhuanthang.ascx.cs`:

1. Load `ThuChi` theo `idChinhanh`, năm/tháng.
2. Tách:
   - `type='1'`: thu
   - `type='0'`: chi
3. Tách doanh thu theo `loai`:
   - `vattu`
   - `dangkykhoahoc`
   - còn lại là thu khác.
4. Tính lợi nhuận:

```text
tongloinhuan = tongthu - tongchi
```

5. Load `Dangky`/`Dangky_group` theo tháng để tính doanh thu khóa học theo danh sách course id hard-coded.

## Logic báo cáo năm

`Baocao_loinhuannam.ascx.cs`:

1. Load `ThuChi` theo năm và chi nhánh.
2. Cộng thu/chi cho từng tháng.
3. Tính tổng thu, tổng chi, tổng lợi nhuận.

## Logic báo cáo lợi nhuận theo khoảng ngày

`LOINHUAN.ascx.cs`:

1. Load `ThuChi` theo khoảng ngày và chi nhánh.
2. Cộng thu/chi/lợi nhuận.
3. Tách theo `loai`:
   - thu đăng ký khóa học
   - chi đơn hàng
   - phí thương hiệu
   - phí đào tạo
   - thu/chi khác.
4. Tách theo `hinhthuc`:
   - `tienmat`
   - `chuyenkhoan`
   - `quetthe`
   - khác.
5. Cộng công nợ từ `Congno`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `ThuChi` | Nguồn chính cho thu/chi/lợi nhuận. |
| `Congno` | Nguồn công nợ cần thu/cần chi. |
| `Dangky`, `Dangky_group` | Doanh thu khóa học theo tháng/course id. |
| `Users` | Hiển thị người tạo phiếu trong view chi tiết. |
| `Hocsinh` | Hiển thị học viên trong công nợ. |

## Tương tác với module khác

- `HOCVIEN` tạo `ThuChi` học phí và `Congno`, ảnh hưởng báo cáo doanh thu/công nợ.
- `Event` tạo `ThuChi` event, ảnh hưởng tổng thu/chi/lợi nhuận.
- `KHO`/vật tư có thể tạo `ThuChi` loại `vattu` hoặc `donhang`.

## Overlap/xung đột

- Báo cáo tháng dùng cả `ThuChi` và `Dangky` để tính các phần khác nhau; số liệu có thể lệch nếu đăng ký đã tạo nhưng chưa thu tiền.
- Course id trong `Baocao_loinhuanthang.ascx.cs` đang hard-coded, ví dụ `105`, `35`, `40`, `44`, `45`, `106`, `81`, `82`, `83`, `84`, `85`, `86`, `101`, `102`.
- Một số view không thấy check permission trực tiếp trong `Page_Load`, phụ thuộc vào route/màn gọi.

## Vấn đề cần sửa

- Làm rõ báo cáo nào tính theo tiền đã thu (`ThuChi`) và báo cáo nào tính theo đăng ký phát sinh (`Dangky`).
- Không hard-code course id trong báo cáo; nên cấu hình theo chương trình/khóa học.
- Sửa lỗi SQL injection: thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query và validate ngày/tháng/năm.
- Tránh query `select *` rồi loop tính toán nếu dữ liệu lớn; nên aggregate bằng SQL.
- Chuẩn hóa quyền truy cập cho các view popup.
