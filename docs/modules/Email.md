# Module Email (`MODULE/Email`)

## Mục đích

`MODULE/Email` quản lý mẫu/nhóm email và màn gửi email. Các file như `Email_cat`, `email_cat_auto`, `Email_Send`, `bangdiem_Column_auto`, `Fillter_auto` cho thấy module này vừa cấu hình danh mục email vừa tạo nội dung gửi theo dữ liệu học viên/bảng điểm.

## Trạng thái sử dụng trong app

Đây là module phụ trợ nhưng có thể ảnh hưởng flow chính nếu các module khác gọi gửi email kết quả, thông báo hoặc bảng điểm.

## File chính

- `MODULE/Email/Email_cat.ascx.cs`
- `MODULE/Email/email_cat_auto.ascx.cs`
- `MODULE/Email/Email_Send.ascx.cs`
- `MODULE/Email/bangdiem_Column_auto.ascx.cs`
- `MODULE/Email/Fillter_auto.ascx.cs`
- `MODULE/Email/Menu_top.ascx`

## DB và dữ liệu liên quan

- Có khả năng dùng bảng danh mục/mẫu email, cột bảng điểm và dữ liệu học viên/lớp.
- Tương tác gián tiếp với `HOCVIEN`, `LOPHOC`, `KHOADAOTAO` và `Event` nếu các module này gửi email.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp ở các endpoint cấu hình.
- Cần chuẩn hóa encode HTML/email body để tránh XSS hoặc email template render sai.
- Cần log trạng thái gửi, người gửi, người nhận và lỗi gửi email.
- Nếu gửi hàng loạt, cần tránh timeout request và có retry/rate limit.

## Ảnh hưởng sang module khác

Sai mẫu email hoặc filter email có thể làm gửi nhầm nội dung/học viên. Khi sửa module này cần test các flow gửi bảng điểm, chứng nhận, event/training notification nếu có dùng chung helper.
