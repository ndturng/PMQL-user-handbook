# Issues module Thi TQ 2024

## Mức ưu tiên cao

- Xác định module còn được dùng hay chỉ là bản legacy/năm cũ.
- Xác định dùng bảng chung hay bảng riêng với `ThiTQ`.
- Sửa lỗi SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Tránh lệch bugfix giữa `ThiTQ` và `ThiTQ_2024`.

## Mức ưu tiên trung bình

- Transaction cho thanh toán/kết quả.
- Audit thay đổi điểm, chứng nhận và thanh toán.
- Validate import, số tiền, chứng từ.
- Kiểm soát quyền HQ/chi nhánh.

## Cần test khi sửa

Nếu module còn active, test cùng bộ case của `ThiTQ` và thêm case dữ liệu năm 2024 không bị ảnh hưởng bởi kỳ thi hiện tại.
