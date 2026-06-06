# Data model module USERS

## Bảng chính

| Bảng | Vai trò |
| --- | --- |
| `Users` | Tài khoản đăng nhập, chi nhánh, nhân sự, active, expire, level. |
| `Nhan_su` | Hồ sơ nhân sự liên kết user. |
| `Chinhanh` | Phạm vi chi nhánh của user. |
| `Permission_join` | Quyền được gán cho user. |
| Bảng permission/template | Danh mục quyền và mẫu quyền. |

## Quan hệ quan trọng

- `Users.idnhansu` -> `Nhan_su.id`
- `Users.idChinhanh` -> `Chinhanh.id`
- `Permission_join.iduser` -> `Users.id`

## Issue DB

- Cần ràng buộc chống nhiều user trùng username hoặc trùng nhân sự nếu nghiệp vụ không cho phép.
- Cần xác định `lever` là level quyền hay loại user, vì nhiều module dựa vào giá trị này.
- Cần chuẩn hóa lưu mật khẩu/hạn dùng.
- Cần audit bảng quyền.
