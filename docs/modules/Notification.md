# Module Notification (`MODULE/Notification`)

## Mục đích

`MODULE/Notification` quản lý danh sách thông báo và plugin hiển thị thông báo quan trọng/thông báo thường.

## Trạng thái sử dụng trong app

Đang được dùng trong flow chính nếu dashboard hoặc popup thông báo load từ module này. Các module khác cũng có thể insert notification khi phát sinh thanh toán, phí hoặc yêu cầu duyệt.

## File chính

- `MODULE/Notification/List_notice.ascx.cs`
- `MODULE/Notification/Plugin_importantnotice.ascx.cs`
- `MODULE/Notification/Plugin_notice.ascx.cs`

## DB và dữ liệu liên quan

- Bảng chính có khả năng là `Notification`.
- Dữ liệu liên quan: `Chinhanh`, `Users`, các module tạo thông báo như `chinhanh`, `KETOAN`, `home`.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với filter/target chi nhánh.
- Cần encode title/content để tránh XSS vì nội dung thông báo có thể chứa text nhập bởi user.
- Cần xác định rule disable/enable notification theo type/value để tránh tắt nhầm thông báo quan trọng.

## Ảnh hưởng sang module khác

Thông báo là kênh phụ trợ cho vận hành. Nếu insert hoặc filter sai, chi nhánh có thể không nhận được yêu cầu thanh toán/xác nhận cần xử lý.
