# Config data text

## Mục đích

Quản lý các cấu hình text/data dùng chung để render nội dung hoặc rule nhỏ trong hệ thống.

File chính:

- `MODULE/Setting/Config_data_text.ascx.cs`
- `MODULE/Setting/html_tes.html`

## DB liên quan

- Bảng cấu hình text/data nếu có.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần encode HTML nếu nội dung được render ra UI.
- Cần audit thay đổi cấu hình.
- Cần document key/value đang được module nào đọc.
