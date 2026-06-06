# Data model module tài liệu

## Bảng chính

- Bảng thư mục tài liệu.
- Bảng file tài liệu.
- Bảng liên kết/chia sẻ nếu có.
- `Users`, `Chinhanh` nếu phân quyền theo user/chi nhánh.

## Issue DB

- Cần xác định rõ ownership: file thuộc user, chi nhánh, folder hay toàn hệ thống.
- Cần constraint chống folder cha trỏ vòng lặp.
- Cần lưu metadata file: tên gốc, tên lưu, size, MIME, extension, người upload, ngày upload.
- Cần cơ chế soft delete hoặc audit cho file đã xóa.
