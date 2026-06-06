# Issues module Kế toán

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Nhiều file nối trực tiếp request/form vào SQL:

- `Phieuthu_Auto.ascx.cs`
- `PhieuChi_Auto.ascx.cs`
- `Nhatky_thuchi.ascx.cs`
- `Hocphi_log.ascx.cs`
- `Baocao_congno.ascx.cs`
- `Baocao_loinhuanthang.ascx.cs`
- `Baocao_loinhuannam.ascx.cs`
- `LOINHUAN.ascx.cs`
- các file trong `MODULE/KETOAN/View/*`
- `MODULE/HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs`

Cần sửa lỗi SQL injection bằng cách thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query, đặc biệt với các field tiền, ngày, id phiếu, chi nhánh, loại phiếu.

### Sinh mã phiếu bằng `count()+1`

Các flow phiếu thu/chi thủ công và học phí sinh mã bằng cách count số dòng `ThuChi` theo chi nhánh rồi cộng 1. Khi nhiều user tạo phiếu cùng lúc, có thể trùng mã.

Nên dùng:

- sequence/table counter có lock
- transaction isolation phù hợp
- hoặc cơ chế giống `Event.GenerateReceiptCodeLocked()` nhưng dùng chung cho toàn hệ thống.

### Flow thu học phí không có transaction tổng

`Phieuthu_hocphi.aspx.cs` insert/update nhiều bảng:

- `ThuChi`
- `Congno`
- `Dangky_group`
- `Dangky`
- `Dangky_thukhac`
- `KHO_Chitietphieu`
- `KHO`
- `Hocsinh_voucher`
- `THITQ`

Nếu lỗi giữa chừng, dữ liệu tiền, trạng thái đăng ký, công nợ và tồn kho có thể lệch.

### Xóa/hủy phiếu không thống nhất

Các cách xử lý hiện tại:

- `Nhatky_thuchi.ascx.cs`: set `ThuChi.tongtien = 0`.
- `Hocphi_log.ascx.cs`: delete thẳng row `Hocphi`.
- Event refund: tạo phiếu chi và update `EventPaymentConfirmation`.
- Học phí: chưa thấy flow hủy phiếu thu hiện đại đồng bộ `ThuChi`/`Congno`.

Cần chuẩn hóa nghiệp vụ hủy phiếu bằng soft delete/audit/refund/reversal, không xóa hoặc set tiền về 0 tùy màn hình.

## Mức ưu tiên trung bình

### `ThuChi` bị dùng bởi nhiều module nhưng thiếu contract rõ

Các module cùng ghi `ThuChi`:

- `KETOAN`: phiếu thủ công.
- `HOCVIEN`: học phí.
- `Event`: thu/hoàn phí event.
- kho/vật tư: vật tư/đơn hàng.

Cần document và enforce contract cho `type`, `loai`, `idphieudangky`, `idhocvien`, `code`, `hinhthuc`.

### `Hocphi` là dữ liệu cũ nhưng vẫn có màn xóa

UI ghi “Phiếu thu học phí (Dữ liệu cũ)”, nhưng vẫn có command xóa row. Cần xác định:

- còn ai đọc bảng `Hocphi`
- có cần khóa màn này ở production không
- có cần migration dữ liệu cũ sang `ThuChi` không

### Báo cáo có thể lệch giữa phát sinh và thực thu

`Baocao_loinhuanthang.ascx.cs` vừa đọc `ThuChi` vừa đọc `Dangky`/`Dangky_group` để tính course revenue. Nếu đăng ký phát sinh nhưng chưa thu đủ, số liệu theo đăng ký và số liệu theo tiền thật có thể khác.

Cần đặt tên rõ:

- doanh thu đã thu
- doanh thu đăng ký/phát sinh
- công nợ còn lại.

### Course id hard-coded trong báo cáo tháng

`Baocao_loinhuanthang.ascx.cs` hard-code nhiều id khóa học/chương trình. Khi cấu hình khóa học thay đổi, báo cáo sẽ sai hoặc thiếu nhóm.

Nên chuyển sang cấu hình theo bảng `Khoahoc`/`Chuongtrinh` hoặc group reporting riêng.

### Permission chưa đồng đều

- Màn chính có check permission.
- Một số `View/*` popup không thấy check permission trực tiếp trong `Page_Load`.
- `Nhatky_thuchi` chỉ hiện remove cho user id `3384`, không phải permission/action rõ ràng.

## Mức ưu tiên thấp nhưng nên dọn

### Build JSON/HTML thủ công

Nhiều response tự nối JSON/HTML và dùng `jsonBase`. Cần chuẩn hóa serialize JSON và encode output.

### Query `select *` rồi loop tính toán

Báo cáo đang query nhiều dòng rồi loop C# để tính tổng. Khi dữ liệu lớn, nên aggregate bằng SQL.

### Kiểu tiền dùng parse chuỗi

Nhiều đoạn parse tiền từ string sau khi replace dấu phẩy/dấu chấm. Nên validate bằng kiểu số rõ ràng và kiểm soát culture.

## Đề xuất hướng sửa

1. Tạo service dùng chung cho phiếu thu/chi:
   - sinh mã phiếu
   - insert `ThuChi`
   - reversal/refund/hủy phiếu
   - validate loại nghiệp vụ.
2. Sửa lỗi SQL injection cho các endpoint kế toán bằng parameterized query.
3. Bọc flow thu học phí trong transaction.
4. Chuẩn hóa contract `ThuChi` và danh mục `loai`.
5. Tách báo cáo “đã thu” khỏi “đăng ký phát sinh”.
6. Rà lại các popup `View/*` để check permission.
7. Khóa hoặc đánh dấu rõ `Hocphi_log` là legacy, tránh xóa dữ liệu cũ thiếu audit.
