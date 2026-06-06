# Issues module Feed

## Mức ưu tiên cao

- Sửa lỗi SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Kiểm soát quyền xem feed theo target.
- Encode/whitelist nội dung feed để tránh XSS.

## Mức ưu tiên trung bình

- Audit thay đổi feed.
- Tối ưu query plugin home.
- Chuẩn hóa overlap với `Notification`.
- Xác định lifecycle: hết hạn, ẩn, đã đọc, xóa.

## Cần test khi sửa

- Tạo feed.
- Gửi feed tới chi nhánh.
- User chi nhánh xem feed.
- User ngoài target không xem được.
- Plugin home hiển thị đúng.
