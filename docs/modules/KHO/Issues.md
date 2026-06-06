# Issues module kho

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Cần sửa lỗi SQL injection bằng parameterized query ở các màn đơn hàng, thống kê, thẻ kho, danh sách vật tư và cập nhật trạng thái.

### Cập nhật tồn kho thiếu transaction

Duyệt đơn có thể insert chi tiết phiếu, update tồn kho và tạo phiếu kế toán. Các thao tác này cần transaction để tránh đơn đã duyệt nhưng tồn/kế toán chưa cập nhật.

### Overlap với `QuanLyKho`

`KHO` và `QuanLyKho` có nhiều file cùng tên. Cần xác định:

- route/menu hiện đang trỏ module nào
- hai module có cùng code hay đã fork khác nhau
- module nào là bản cần sửa khi có bug kho

## Mức ưu tiên trung bình

- Chống duyệt đơn nhiều lần.
- Validate số lượng, đơn giá, hệ số quy đổi.
- Chuẩn hóa rule tồn kho âm.
- Index các cột lọc báo cáo: chi nhánh, vật tư, ngày, trạng thái.
- Encode output tên vật tư/ghi chú.

## Cần test khi sửa

- Tạo đơn hàng.
- Duyệt/từ chối đơn.
- Cập nhật tồn kho.
- Báo cáo thẻ kho.
- Báo cáo chi tiết theo chi nhánh/ngày.
