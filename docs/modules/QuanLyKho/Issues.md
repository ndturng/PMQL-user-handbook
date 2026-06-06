# Issues module `QuanLyKho`

## Mức ưu tiên cao

- Xác định `QuanLyKho` có còn được dùng trong flow chính hay không.
- Xử lý overlap với `KHO`.
- Sửa lỗi SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Bọc transaction cho cập nhật đơn hàng/tồn kho/kế toán.

## Mức ưu tiên trung bình

- Chuẩn hóa quyền giữa menu và endpoint.
- Kiểm tra chống duyệt đơn lặp.
- Validate số lượng/đơn giá/hệ số quy đổi.
- Chuẩn hóa công thức báo cáo với `KHO`.

## Cần test khi sửa

Test cùng bộ case với `KHO`, sau đó xác nhận route/menu thực tế không còn trỏ nhầm bản cũ.
