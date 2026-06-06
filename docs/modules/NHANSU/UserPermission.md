# Liên kết nhân sự với user và phân quyền

## Feature

Feature này mô tả cách `Nhan_su` liên kết với `Users` và quyền hệ thống. Đây là phần quan trọng vì nhân sự không chỉ là hồ sơ, mà còn có thể có tài khoản đăng nhập.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Thêm nhân sự từ `Add_nhansu.ascx.cs` có tạo `Users`.
- Danh sách nhân sự cảnh báo nếu nhân sự thiếu user, user không active hoặc có nhiều user.
- JS trong `MODULE/NHANSU/js/jsnhansu.js` mở popup module `USERS` để thêm/sửa tài khoản theo nhân sự.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/NHANSU/Add_nhansu.ascx.cs` | Insert `Nhan_su`, insert `Users`, gán permission template. |
| `MODULE/NHANSU/Add_giaovien_auto.ascx.cs` | Khi sửa nhân sự, sync họ tên/level/username sang `Users`. |
| `MODULE/NHANSU/Danhsach.ascx.cs` | Cảnh báo quan hệ `Nhan_su` - `Users`. |
| `MODULE/NHANSU/js/jsnhansu.js` | Mở `users!Users_Add_byns`, `users!Users_Add`. |
| `MODULE/USERS/*` | Quản lý tài khoản/quyền liên quan nhân sự. |

## Logic tạo tài khoản từ nhân sự

`Add_nhansu.ascx.cs`:

1. Insert `Nhan_su`.
2. Xác định `lever` theo `vitri` bằng `Users.GetLeverByRole(vitri)`.
3. Insert `Users` với:
   - `hoten`
   - `idnhansu`
   - `enable=1`
   - `lever`
   - `idChinhanh`
   - `active=1`
   - `inputuser`
   - `inputpass` mặc định dạng hash cố định
   - `isTraining=1`
   - `DateStartWork`
4. Nếu có permission template theo role, gọi `Users.Permission_ByTemplate(newUserId, vitri, userLever)`.

## Logic sinh username

Username được sinh theo công thức:

```text
gv_ + tên cuối không dấu + 3 số cuối điện thoại
```

Nếu trùng và điện thoại đủ dài, thử 4 số cuối. Nếu vẫn trùng thì báo lỗi.

## Logic sync khi sửa nhân sự

`Add_giaovien_auto.ascx.cs`:

- Khi sửa `Nhan_su`, code có sync sang `Users` theo `idnhansu`:
  - `hoten`
  - `lever`
  - `inputuser`
  - `DateStartWork`

## Cảnh báo quan hệ user

`Danhsach.ascx.cs` build map cảnh báo:

- Nhân sự không có thông tin user.
- Nhân sự gắn với nhiều user.
- User của nhân sự không active.

Cảnh báo này chỉ hiện nếu `Users.IsAdminHQ()`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Nhan_su` | Hồ sơ nhân sự. |
| `Users` | Tài khoản đăng nhập. |
| `Permission_join` | Quyền user. |
| `Config_data_text` | Vị trí/level dùng tính role. |

## Tương tác với module khác

- `USERS` quản lý tài khoản/quyền sau khi tạo từ nhân sự.
- `HOCVIEN`, `KETOAN`, `NHANSU` thống kê theo `Users.id`.
- `LOPHOC` dùng `Nhan_su.id`, không dùng trực tiếp `Users.id`.

## Overlap/xung đột

- Có thể tạo/sửa user từ module `USERS`, trong khi `NHANSU` cũng sync một số field sang `Users`.
- Nếu một nhân sự có nhiều user hoặc user bị disable, thống kê/doanh số có thể lệch.
- Xóa mềm nhân sự không đồng bộ rõ với `Users.enable`.

## Vấn đề cần sửa

- SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Insert `Nhan_su` và `Users` cần transaction.
- Cần chuẩn hóa rule disable: xóa nhân sự có disable user hay không.
- Mật khẩu mặc định/hash cố định cần được audit lại về bảo mật.
- Nên có constraint hoặc rule rõ: một `Nhan_su` chỉ gắn một active `Users`.
