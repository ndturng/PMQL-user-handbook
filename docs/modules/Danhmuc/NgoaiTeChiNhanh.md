# Ngoại tệ và chi nhánh trong danh mục

## Mục đích

Nhóm này quản lý danh mục ngoại tệ và một số danh mục chi nhánh/cơ sở.

File chính:

- `MODULE/Danhmuc/ngoaite.ascx.cs`
- `MODULE/Danhmuc/chinhanh.ascx.cs`

## DB liên quan

- Bảng ngoại tệ/tỷ giá.
- `Chinhanh`.

## Vấn đề cần chú ý

- Cần xác định overlap với `home/ngoaite` và `chinhanh`.
- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Tỷ giá/ngoại tệ nếu ảnh hưởng kế toán cần audit và ngày hiệu lực.
- Chi nhánh nên sửa ở source of truth chính để tránh lệch dữ liệu.
