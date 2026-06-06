# HOCVIEN: Hồ sơ học viên

## Mục đích

Nhóm hồ sơ học viên chịu trách nhiệm thêm mới, sửa và xem chi tiết học viên.

File chính:

- `MODULE/HOCVIEN/Addnew_auto.ascx`
- `MODULE/HOCVIEN/Addnew_auto.ascx.cs`
- `MODULE/HOCVIEN/View_Detail_auto.ascx`
- `MODULE/HOCVIEN/View_Detail_auto.ascx.cs`
- `MODULE/HOCVIEN/edit_hocvien.ascx`
- `MODULE/HOCVIEN/add_hocvien.ascx`
- `App_Code/BO/Hocvien.cs`
- `App_Code/BO/Form_auto.cs`

## Trạng thái sử dụng trong app

Đây là flow chính để tạo/sửa hồ sơ. `Menu_top.ascx` gọi:

```text
Frame-ajax.aspx?mod=hocvien!addnew_auto
```

Khi sửa, form được mở với `idedit`.

## Cơ chế form động

`Addnew_auto.ascx.cs` dùng:

```csharp
Form_auto.List_Column("Hocsinh")
Form_auto.Detack_text(...)
```

Điều này nghĩa là form không hard-code toàn bộ field trong code-behind. Nhiều input được sinh từ metadata của bảng/cấu hình `Hocsinh`.

Prefix field:

```text
zxhocvien_
```

Với `View_Detail_auto`, prefix là:

```text
zhocvien_
```

## Luồng thêm mới

1. User bấm "Thêm mới" từ `Menu_top.ascx`.
2. Mở colorbox `hocvien!addnew_auto`.
3. `Page_Load` kiểm tra quyền `01HV`, action `2`.
4. `Load_new_form()` sinh form từ `Form_auto`.
5. Submit postback với `ctl10$cmd=addnew`.
6. `POST_Insert()` đọc metadata column và build SQL insert vào `Hocsinh`.
7. Sau insert, code tạo `MaHS` theo mã chi nhánh và id học viên.

## Luồng sửa hồ sơ

1. Mở form với `idedit`.
2. `Page_Load` kiểm tra quyền `01HV`, action `3`.
3. `Load_edit_form(idedit)` load dữ liệu từ `Hocsinh`.
4. Submit postback với `ctl10$cmd=editform`.
5. `POST_Edit()` build SQL update theo metadata.
6. Một số bản code dùng `UpdateStudentInTransaction(...)` và ghi log.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Hocsinh` | Hồ sơ học viên |
| `Users` | Người tạo/sửa |
| `ChiNhanh` | Mã chi nhánh, id chi nhánh học viên |
| `Nhansu_Log` | Log thay đổi ở một số flow |
| Metadata của `Form_auto` | Sinh field/form/filter |

## Field/lớp học đặc biệt

`Addnew_auto.ascx.cs` có mapping lớp học theo enum nội bộ:

| Value | Label |
| --- | --- |
| `-2` | Mầm |
| `-1` | Chồi |
| `0` | Lá |
| `1` - `5` | Lớp 1 - lớp 5 |
| `6` | Lớp 5+ |

Các field này cần kiểm tra kỹ khi thay đổi schema hoặc UI vì có xử lý riêng trong code.

## Vấn đề cần chú ý

- SQL insert/update được build bằng string concat từ form.
- Form động phụ thuộc metadata `Form_auto`; đổi schema hoặc description có thể ảnh hưởng UI.
- Có nhiều file thêm/sửa hồ sơ cũ và mới (`add_hocvien`, `Addnew_auto`, `edit_hocvien`), cần xác định file nào đang được menu gọi trước khi sửa.
- Upload ảnh trong `Addnew_auto` cần kiểm tra validate extension/size/path nếu sửa.
- Mã học viên được tạo sau insert; cần tránh race/trùng mã nếu nhiều user thêm cùng lúc.
- `View_Detail_auto.ascx.cs` cũng chứa code insert/update cũ dù chủ yếu dùng để xem, nên cần tránh sửa nhầm flow không còn dùng.
