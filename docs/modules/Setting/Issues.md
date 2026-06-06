# Issues module Setting

## Mức ưu tiên cao

- Sửa lỗi SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Audit thay đổi cấu hình nhạy cảm.
- Xác định source of truth giữa `Setting` và `THIETLAP`.

## Mức ưu tiên trung bình

- Validate ngày, số tiền, phần trăm giảm, scope chi nhánh.
- Encode nội dung config text.
- Document module nào đang đọc từng config key.
- Kiểm tra quyền thao tác cấu hình.

## Cần test khi sửa

- Tạo/sửa/xóa khuyến mãi.
- Tạo/sửa/xóa lịch nghỉ.
- Cập nhật config text.
- Kiểm tra đăng ký/học phí/lịch học sau khi đổi setting.
