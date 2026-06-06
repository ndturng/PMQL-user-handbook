# Issues module Thi TQ

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Cần sửa lỗi SQL injection bằng parameterized query ở đăng ký, thanh toán, xác nhận, setup câu hỏi/xếp hạng, import và thống kê.

### Overlap với `ThiTQ_2024`

Hai module có footprint gần giống nhau. Cần xác định:

- module nào đang dùng cho kỳ thi hiện tại
- dữ liệu dùng chung hay tách theo năm
- sửa bug có cần áp dụng cả hai module không

### Thanh toán/kết quả cần transaction và audit

Flow tài chính và flow kết quả thi đều nhạy cảm. Cần audit người thao tác, trạng thái trước/sau và chống thao tác lặp.

## Mức ưu tiên trung bình

- Validate file import.
- Validate số tiền/chiết khấu/số lượng.
- Kiểm soát quyền HQ/chi nhánh.
- Encode output tên thí sinh/ghi chú.
- Tối ưu thống kê.

## Cần test khi sửa

- Đăng ký thí sinh/nhóm.
- Thanh toán và xác nhận.
- Setup câu hỏi/xếp hạng.
- Import số báo danh/tên.
- In chứng nhận.
- Thống kê theo khu vực/bảng/chi nhánh.
