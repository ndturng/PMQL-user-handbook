# Module USERS (`MODULE/USERS`)

## Mục đích

`MODULE/USERS` quản lý tài khoản đăng nhập, trạng thái active/expire, đổi mật khẩu, phân quyền, template quyền và liên kết user với nhân sự/chi nhánh.

## Trạng thái sử dụng trong app

Đây là module hạ tầng của flow chính. Hầu hết module nghiệp vụ đều dựa vào `Users`, session user, `idChinhanh`, `lever` và permission key.

## Tài liệu con

- [TaiKhoan.md](./TaiKhoan.md): danh sách user, thêm/sửa, active/expire, đổi mật khẩu.
- [PhanQuyen.md](./PhanQuyen.md): permission, template, phân quyền.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro và việc cần sửa.

## File liên quan

- `MODULE/USERS/userlist.ascx.cs`
- `MODULE/USERS/Users_Add.ascx.cs`
- `MODULE/USERS/Users_Add_byns.ascx.cs`
- `MODULE/USERS/ChangePass.ascx.cs`
- `MODULE/USERS/acount-permision.ascx.cs`
- `MODULE/USERS/Permission_add.ascx.cs`
- `MODULE/USERS/chinhanh.ascx.cs`
- `MODULE/USERS/Exchange_nhansu.ascx.cs`
- `MODULE/USERS/info_logon.ascx.cs`

## Overlap

- `NHANSU` tạo/cập nhật user từ nhân sự.
- `KHOADAOTAO` có thể tạo/kích hoạt user sau tập huấn.
- `chinhanh` có danh sách user chi nhánh.
- `THIETLAP` có permission template.

## Nhận xét nhanh

Đây là module nhạy cảm về bảo mật. Mọi sửa đổi cần kiểm tra quyền, active, hết hạn, mật khẩu và liên kết `Users.idnhansu`.
