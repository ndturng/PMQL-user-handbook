# Issues module chi nhánh

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Nhiều endpoint lấy query string rồi nối vào SQL. Cần sửa lỗi SQL injection bằng parameterized query, nhất là các flow:

- thêm/sửa/xóa chi nhánh/cụm
- xác nhận học viên
- thanh toán phí/mua thêm tài khoản
- update hạn dùng/số lượng tài khoản
- danh sách user/học viên/tính tiền

### Thiếu transaction trong flow thanh toán

Thanh toán có thể vừa insert `Thanhtoan_User`, vừa insert/update `ThuChi`, vừa update `Chinhanh`, `Users`, `Hocsinh`, `Notification`. Nếu một bước lỗi, dữ liệu có thể lệch.

### Overlap với kế toán và user

`chinhanh` tự xử lý một phần thanh toán/user, trong khi `KETOAN` và `USERS` cũng quản lý các domain đó. Cần xác định rõ quyền sở hữu logic để tránh sửa một nơi nhưng báo cáo nơi khác không khớp.

## Mức ưu tiên trung bình

- Validate file upload chứng từ thanh toán: extension, MIME, size, tên file, quyền upload.
- Kiểm tra permission duyệt/xác nhận riêng với permission xem.
- Chuẩn hóa audit log cho thay đổi hạn dùng, active, số lượng tài khoản.
- Encode output khi render thông báo, ghi chú, tên chi nhánh.

## Cần test khi sửa

- Tạo/sửa chi nhánh.
- Thanh toán phí hệ thống.
- Mua thêm tài khoản.
- Xác nhận học viên.
- Danh sách user/học viên theo chi nhánh.
- Báo cáo kế toán liên quan phí chi nhánh.
