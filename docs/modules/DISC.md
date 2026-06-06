# Module DISC (`MODULE/DISC`)

## Mục đích

`MODULE/DISC` là module rất nhỏ, hiện chỉ thấy `Test.ascx` và `main.js`. Chưa thấy dấu hiệu đây là flow vận hành chính của app.

## Trạng thái sử dụng trong app

Nên xem đây là module thử nghiệm hoặc feature phụ cho đến khi xác nhận được link/menu đang trỏ vào `disc`.

## DB và tương tác

Chưa thấy footprint DB rõ ràng. Nếu module có dùng dữ liệu, khả năng nằm trong JavaScript hoặc file test.

## Vấn đề cần chú ý

- Cần xác nhận có route/menu dùng thật hay không trước khi đầu tư sửa.
- Nếu có xử lý input trong JS, cần kiểm tra encoding/XSS.
- Nếu không còn dùng, nên cân nhắc loại khỏi menu hoặc đánh dấu legacy.
