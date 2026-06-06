# Hồ sơ nhân sự

## Feature

Feature này quản lý danh sách hồ sơ nhân sự/giáo viên, thêm/sửa, xóa mềm, xác nhận chính thức, chuyển cơ sở và import.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Entry: `Default.aspx?mod=nhansu!danhsach`
- Dashboard: `Default.aspx?mod=nhansu!home`
- Permission chính: `04NS` hoặc `04NS ` trong code.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/NHANSU/Danhsach.ascx.cs` | Load danh sách nhân sự, filter, xóa mềm, xác nhận chính thức. |
| `MODULE/NHANSU/Add_nhansu.ascx.cs` | Thêm nhân sự cơ bản và tạo tài khoản user. |
| `MODULE/NHANSU/Add_giaovien_auto.ascx.cs` | Form động thêm/sửa nhân sự, sync sang `Users`, upload file. |
| `MODULE/NHANSU/thongtin_gv.ascx.cs` | Xem thông tin giáo viên/nhân sự. |
| `MODULE/NHANSU/Exchange_nhansu.ascx.cs` | Chuyển cơ sở nhân sự. |
| `MODULE/NHANSU/importList.aspx.cs`, `importList_new.aspx.cs` | Import danh sách nhân sự. |
| `MODULE/NHANSU/CAPNHAT_thamnien.ascx.cs` | Cập nhật thâm niên. |
| `MODULE/NHANSU/Capnhat.ascx.cs` | Màn cập nhật/danh sách dạng khác, overlap với `Danhsach`. |

## Logic danh sách

`Danhsach.ascx.cs`:

1. Check permission `04NS ` action `1`.
2. Load chi nhánh user được phép xem.
3. Load vị trí nhân sự từ `Config_data_text` type `4`.
4. `cmd=load` filter theo:
   - `idchinhanh`
   - `vitri`
   - `loai`/`temp`
   - `search`
5. Query `Nhan_su.enable=1`.
6. Render:
   - họ tên
   - điện thoại
   - email
   - số giấy chứng nhận
   - ngày cấp/ngày hết hạn chứng nhận
   - thâm niên
   - các thao tác sửa, xóa, cập nhật thâm niên, chuyển cơ sở, xác nhận chính thức.

Danh sách còn cảnh báo nhân sự thiếu user, user không active hoặc nhân sự gắn nhiều user nếu user hiện tại là admin HQ.

## Logic thêm nhân sự

`Add_nhansu.ascx.cs`:

1. Check permission `04NS`, action `2`.
2. Validate tên, email, điện thoại.
3. Sinh username dạng:

```text
gv_{tenkhongdau}{3 hoặc 4 số cuối điện thoại}
```

4. Kiểm tra trùng email trong `Nhan_su`.
5. Kiểm tra trùng họ tên + điện thoại.
6. Insert `Nhan_su`.
7. Insert `Users` gắn `idnhansu`.
8. Gán quyền theo template role nếu có `Users.HasTemplate(vitri)`.

## Logic sửa nhân sự

`Add_giaovien_auto.ascx.cs`:

- Dùng `Form_auto` để sinh form theo metadata bảng `Nhan_su`.
- Khi sửa, update `Nhan_su`, sau đó sync sang `Users`:
  - `hoten`
  - `lever`
  - `inputuser`
  - `DateStartWork`
- Có validate email trùng và họ tên + điện thoại trùng.
- Có logic chứng nhận qua `GCN_Nhansu`.

## Logic xóa/xác nhận

`Danhsach.ascx.cs`:

- `cmd=delete`: set `Nhan_su.enable='0'`.
- `cmd=xacnhan`: set `Nhan_su.temp='0'`.

Lưu ý: có chỗ permission key là `04NS ` có khoảng trắng dư.

## Logic chuyển cơ sở

`Exchange_nhansu.ascx.cs`:

1. Check level user.
2. Load nhân sự hiện tại, chi nhánh hiện tại, chi nhánh đích.
3. Khi update:
   - update `Nhan_su.idchinhanh`, `vitri`, `ghichu`.
   - update `Users.idchinhanh` theo `idnhansu`.
   - update `Lop_TKB.idgiaovien` từ nhân sự cũ sang nhân sự thay thế.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Nhan_su` | Hồ sơ nhân sự chính. |
| `Users` | Tài khoản đăng nhập liên kết nhân sự. |
| `Permission_join` | Quyền user được tạo/gán template. |
| `Config_data_text` | Danh mục vị trí/cấp bậc. |
| `ChiNhanh` | Chi nhánh nhân sự. |
| `GCN_Nhansu` | Giấy chứng nhận nhân sự. |
| `Danhgia_taphuan` | Kết quả đánh giá/tập huấn, fallback cho chứng nhận. |
| `Lop_TKB` | Bị update khi chuyển giáo viên/cơ sở. |

## Overlap/xung đột

- `Danhsach.ascx.cs`, `Capnhat.ascx.cs`, `Add_nhansu.ascx.cs`, `Add_giaovien_auto.ascx.cs` cùng liên quan hồ sơ nhân sự.
- `Add_nhansu` tạo `Users`, còn `Add_giaovien_auto` khi insert chỉ tạo `Nhan_su`; cần biết UI nào đang là flow chính.
- Chuyển cơ sở update luôn `Lop_TKB`, có thể làm thay đổi lịch lớp hiện hữu.

## Vấn đề cần sửa

- SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Thêm nhân sự và tạo `Users` không được bọc transaction đầy đủ.
- Permission key có khoảng trắng dư: `04NS `.
- Xóa nhân sự chỉ disable `Nhan_su`, chưa thấy đồng bộ disable `Users`.
- Chuyển cơ sở update `Lop_TKB` hàng loạt, cần log/audit và transaction.
