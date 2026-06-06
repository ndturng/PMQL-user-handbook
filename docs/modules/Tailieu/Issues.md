# Issues module tài liệu

## Mức ưu tiên cao

- Kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Bảo mật upload/download file: extension, MIME, size, path traversal.
- Kiểm soát quyền xem file/folder/share link.
- Chống IDOR khi truy cập file/folder bằng id.

## Mức ưu tiên trung bình

- Audit upload/sửa/xóa/chia sẻ.
- Encode tên file/thư mục/ghi chú.
- Chuẩn hóa nơi lưu file và cleanup file mồ côi khi DB rollback/xóa.
- Version hóa hoặc lịch sử thay đổi nếu tài liệu quan trọng.

## Cần test khi sửa

- Tạo/sửa thư mục.
- Upload/sửa file.
- Xem file/folder.
- Tạo link chia sẻ.
- User không có quyền truy cập link/id tài liệu.
