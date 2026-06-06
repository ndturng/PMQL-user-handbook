# Phân quyền user

## Mục đích

Nhóm này quản lý permission của tài khoản, template quyền và liên kết user với chi nhánh/nhân sự.

File chính:

- `MODULE/USERS/acount-permision.ascx.cs`
- `MODULE/USERS/Permission_add.ascx.cs`
- `MODULE/USERS/chinhanh.ascx.cs`
- `MODULE/USERS/Exchange_nhansu.ascx.cs`
- `MODULE/THIETLAP/PermissionTemplate.ascx.cs`

## Logic chính

- Gán permission key cho user.
- Hiển thị/sửa quyền thao tác.
- Gắn user với chi nhánh.
- Chuyển liên kết nhân sự/user.
- Dùng template quyền để cấp nhanh.

## DB liên quan

- `Permission_join`
- bảng permission/template quyền.
- `Users`
- `Nhan_su`
- `Chinhanh`

## Vấn đề cần chú ý

- Menu chỉ là lớp hiển thị; endpoint vẫn phải kiểm tra quyền.
- Cần chuẩn hóa permission key vì nhiều module dùng key khác nhau và có typo/khoảng trắng.
- Cần audit mọi thay đổi quyền.
- Cần tránh user tự sửa quyền hoặc sửa quyền user có cấp cao hơn.
