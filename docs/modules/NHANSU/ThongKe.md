# Thống kê nhân sự

## Feature

Feature này thống kê hoạt động/doanh số/doanh thu theo nhân sự hoặc user liên kết nhân sự.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Thống kê nhân sự: `Default.aspx?mod=nhansu!thongke_doanhso`
- Thống kê doanh thu: `Default.aspx?mod=nhansu!thongke_doanhthu`
- Lịch sử thao tác: `Default.aspx?mod=nhansu!LICHSU_Thaotac`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/NHANSU/Thongke_doanhSo.ascx.cs` | Thống kê học viên mới, tái ký, không tái ký, hóa đơn. |
| `MODULE/NHANSU/THONGKE_Doanhthu.ascx.cs` | Thống kê doanh thu theo user/nhân sự. |
| `MODULE/NHANSU/DoanhSo_ChiTiet.ascx.cs` | Chi tiết doanh số. |
| `MODULE/NHANSU/LICHSU_Thaotac.ascx.cs` | Lịch sử thao tác/phiếu đăng ký. |
| `MODULE/HOCVIEN/*` | Nguồn học viên/đăng ký. |
| `MODULE/KETOAN/*` | Nguồn học phí/doanh thu. |

## Logic thống kê doanh số

`Thongke_doanhSo.ascx.cs`:

1. Check permission `04TK`.
2. Load user/nhân sự theo chi nhánh:
   - join `Users` và `Nhan_su`
   - `Users.enable=1`
   - `Nhan_su.vitri='1'`
3. Với từng user:
   - `count_hocvienCreate`: số học viên mới từ `Hocsinh`.
   - `count_hocvienLenlop`: số học viên hết hạn/tái ký từ `Lophoc_join`.
   - `count_chothoadon`: số phiếu `Dangky_group` theo trạng thái.
4. Render bảng tổng hợp.

## Logic thống kê doanh thu

`THONGKE_Doanhthu.ascx.cs`:

1. Check permission `04TKDT`.
2. Load `Users` active theo chi nhánh và có `idnhansu`.
3. Với từng user:
   - tính doanh số từ `Dangky_group` join `Hocphi`.
   - đếm phiếu đã thu tiền.
   - đếm `Dangky_thukhac`.
4. Có link mở popup danh sách phiếu đăng ký theo user/khoảng ngày.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Users` | User tạo học viên/đăng ký, liên kết nhân sự. |
| `Nhan_su` | Hồ sơ nhân sự. |
| `Hocsinh` | Học viên mới. |
| `Lophoc_join` | Tái ký/hết hạn khóa học. |
| `Dangky_group` | Phiếu đăng ký và trạng thái hóa đơn. |
| `Dangky_thukhac` | Thu khác theo đăng ký. |
| `Hocphi` | Doanh thu/học phí cũ trong thống kê doanh thu. |

## Tương tác với module khác

- `HOCVIEN` tạo học viên và phiếu đăng ký.
- `LOPHOC` tạo `Lophoc_join`.
- `KETOAN`/học phí ảnh hưởng số liệu doanh thu.
- `USERS` ảnh hưởng số liệu vì thống kê dựa vào `Users.id`.

## Overlap/xung đột

- Thống kê doanh thu dùng `Hocphi`, trong khi docs `KETOAN` xác định `Hocphi` có vẻ là dữ liệu cũ và flow mới dùng `ThuChi`.
- Thống kê dựa vào `Users.id`, không phải trực tiếp `Nhan_su.id`; nếu user - nhân sự lệch, báo cáo sai.
- “Tái ký/không tái ký” đang suy luận từ `Lophoc_join.todate`, có thể khác logic trong `LOPHOC`.

## Vấn đề cần sửa

- SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Làm rõ thống kê doanh thu dùng `Hocphi` hay `ThuChi`.
- Cần đồng bộ với contract user - nhân sự để tránh lệch số liệu.
- Nên aggregate bằng SQL thay vì query từng user rồi count từng chỉ số.
- Chuẩn hóa định nghĩa “tái ký”, “không tái ký”, “đã chốt hóa đơn”.
