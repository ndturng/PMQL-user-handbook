# Data model module Setting

## Bảng chính

- Bảng khuyến mãi.
- Bảng lịch nghỉ/ngày nghỉ.
- Bảng config data text.

## Quan hệ/tác động

- Khuyến mãi ảnh hưởng học phí/đăng ký/kế toán.
- Lịch nghỉ ảnh hưởng lớp học/thời khóa biểu nếu module lớp đọc dữ liệu này.
- Config text ảnh hưởng UI/email/thông báo tùy key.

## Issue DB

- Cần xác định tên bảng và key chính xác cho từng nhóm setting.
- Cần audit version hoặc lịch sử thay đổi.
- Cần constraint chống trùng key/config hoặc trùng lịch nghỉ.
