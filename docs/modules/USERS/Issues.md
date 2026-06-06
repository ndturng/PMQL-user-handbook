# Issues module USERS

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Cần sửa lỗi SQL injection bằng parameterized query ở danh sách user, thêm user, phân quyền, đổi trạng thái, chuyển nhân sự/chi nhánh.

### Bảo mật mật khẩu và active

Một số flow trong repo có dấu hiệu reset mật khẩu mặc định hoặc kích hoạt user từ module khác. Cần kiểm tra hash, policy mật khẩu, audit và quyền thao tác.

### Phân quyền không được chỉ dựa vào menu

Ẩn menu không đủ. Mọi endpoint cần check permission server-side.

## Mức ưu tiên trung bình

- Chuẩn hóa permission key.
- Audit thay đổi quyền/active/expire/password.
- Kiểm soát scope HQ/chi nhánh.
- Encode output tên user/ghi chú.

## Cần test khi sửa

- Login user chi nhánh và HQ.
- Thêm user mới.
- Thêm user từ nhân sự.
- Đổi mật khẩu.
- Toggle active/expire.
- Gán quyền và truy cập module tương ứng.
