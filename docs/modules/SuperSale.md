# Module SuperSale (`MODULE/SuperSale`)

## Mục đích

`MODULE/SuperSale` hiện rất nhỏ, chỉ thấy flow `SubmitOrder`. Có khả năng là endpoint nhận đơn hoặc thử nghiệm bán hàng.

## Trạng thái sử dụng trong app

Chưa thấy bằng chứng đây là flow chính. Cần xác nhận route/link trước khi mở rộng.

## File chính

- `MODULE/SuperSale/SubmitOrder.aspx.cs`

## DB và dữ liệu liên quan

Có khả năng ghi đơn hàng hoặc thông tin khách hàng. Cần kiểm tra bảng thực tế trong code trước khi sửa production.

## Vấn đề cần chú ý

- Nếu endpoint public hoặc nhận form, cần kiểm tra xác thực, CSRF và validate input.
- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp nếu có ghi DB từ request.
- Cần log lỗi và trạng thái submit đơn để tránh mất đơn.

## Ảnh hưởng sang module khác

Nếu đơn hàng được chuyển sang kế toán/kho/học viên, cần map rõ bảng trung gian. Nếu không dùng, nên đánh dấu legacy.
