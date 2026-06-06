# Module Thi TQ 2024 (`MODULE/ThiTQ_2024`)

## Mục đích

`MODULE/ThiTQ_2024` là biến thể theo năm của module `ThiTQ`, có nhiều file cùng tên và cùng domain: đăng ký, thanh toán, câu hỏi, xếp hạng, kết quả, chứng nhận và thống kê.

## Trạng thái sử dụng trong app

Cần xác nhận production còn dùng module này cho dữ liệu năm 2024 hay đã chuyển sang `ThiTQ`. Không nên sửa như module chính nếu chưa kiểm tra route/menu.

## Tài liệu con

- [DangKyThanhToan.md](./DangKyThanhToan.md): đăng ký và thanh toán.
- [CauHoiKetQua.md](./CauHoiKetQua.md): câu hỏi, xếp hạng, kết quả, chứng nhận.
- [ThongKe.md](./ThongKe.md): thống kê.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro và việc cần sửa.

## Overlap

Overlap trực tiếp với `MODULE/ThiTQ`. Rủi ro lớn nhất là hai module cùng logic nhưng chỉ một bên được sửa lỗi.

## Nhận xét nhanh

Nếu đây là bản lưu dữ liệu năm 2024, nên giữ ổn định và chỉ sửa lỗi bảo mật/nghiêm trọng. Nếu đây là bản copy còn dùng, cần hợp nhất hoặc document rõ route theo năm.
