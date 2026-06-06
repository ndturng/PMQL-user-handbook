# Tài khoản user

## Mục đích

Nhóm này quản lý danh sách tài khoản, thêm user, thêm user theo nhân sự, đổi mật khẩu, active/expire và thông tin đăng nhập.

File chính:

- `MODULE/USERS/userlist.ascx.cs`
- `MODULE/USERS/Users_Add.ascx.cs`
- `MODULE/USERS/Users_Add_byns.ascx.cs`
- `MODULE/USERS/ChangePass.ascx.cs`
- `MODULE/USERS/info_logon.ascx.cs`
- `MODULE/USERS/checkout.ascx.cs`

## Logic chính

- Load danh sách user theo chi nhánh/quyền.
- Toggle active.
- Tăng hạn dùng/expire.
- Xóa hoặc disable tài khoản.
- Tạo user mới hoặc tạo user từ `Nhan_su`.
- Đổi mật khẩu.

## DB liên quan

- `Users`
- `Nhan_su`
- `Chinhanh`

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với id user, chi nhánh, filter.
- Cần chuẩn hóa hash mật khẩu và tránh reset password mặc định không kiểm soát.
- Toggle active/expire phải có audit.
- Cần kiểm tra rule `lever`/HQ để user chi nhánh không thao tác user ngoài scope.
