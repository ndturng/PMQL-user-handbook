# Phòng học

## Feature

Feature này quản lý danh mục phòng học theo chi nhánh. Phòng học được dùng khi cấu hình thời khóa biểu lớp.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Entry: `lophoc!phonghoc`
- Permission xem: `03PH`
- Permission thêm: `03PH`, action `2`
- Permission sửa: `03PH`, action `3`
- Permission xóa: `03PH`, action `4`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/LOPHOC/Phonghoc.ascx.cs` | Load danh sách phòng học, xóa mềm phòng. |
| `MODULE/LOPHOC/phonghoc_auto.ascx.cs` | Form động thêm/sửa phòng học. |
| `MODULE/LOPHOC/Lophoc_TKB.ascx.cs` | Sử dụng phòng học khi cấu hình lịch lớp. |

## Logic chính

`Phonghoc.ascx.cs` xử lý:

- `Load_list`: load danh sách phòng học theo chi nhánh.
- `delph`: xóa mềm phòng bằng `PhongHoc.enable = 0`.

`phonghoc_auto.ascx.cs` dùng `Form_auto` với bảng `PhongHoc` và prefix `zPhongHoc_` để thêm/sửa dữ liệu.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `PhongHoc` | Danh mục phòng học. |
| `Lop_TKB` | Lịch lớp tham chiếu tới phòng học. |

## Tương tác với module khác

- `ThoiKhoaBieu` dùng `PhongHoc` để chọn phòng cho từng buổi học.
- Nếu xóa mềm phòng đang được dùng trong `Lop_TKB`, lịch cũ vẫn có thể còn tham chiếu tới phòng đó.

## Overlap/xung đột

- Danh mục phòng học chỉ thấy nằm trong `LOPHOC`, không thấy module trùng trực tiếp.
- Xung đột chính là dữ liệu phòng với lịch lớp: phòng bị xóa mềm nhưng vẫn được tham chiếu trong lịch.

## Vấn đề cần sửa

- **SQL injection do truy vấn SQL nối chuỗi trực tiếp**.
- Cần kiểm tra trước khi xóa mềm phòng đang được dùng trong `Lop_TKB`.
- Chưa thấy cơ chế kiểm tra trùng phòng theo cùng khung giờ.
