# Thu phí và hoàn phí event

## Feature

Feature này xử lý thu phí event, hoàn phí event và đồng bộ dữ liệu giữa `EventPaymentConfirmation` với `ThuChi`.

## Trạng thái trong flow chính

Có dùng trong flow chính cho event có phí (`Event.Price > 0`).

- Entry UI: modal thu/hoàn phí trong `MODULE/Event/List.ascx`.
- Code chính: `MODULE/Event/List.ascx.cs`.
- JS chính: `MODULE/Event/js/event.js`.

## Function/command liên quan

| Command/function | Vai trò |
| --- | --- |
| `cmd=create_payment` / `CreateEventPayment()` | Tạo yêu cầu thu phí. |
| `CreateEventPaymentAtomic()` | Insert payment confirmation và phiếu thu `ThuChi` trong transaction. |
| `cmd=refund_payment` / `RefundPayment()` | Tạo yêu cầu hoàn phí. |
| `RefundPaymentAtomic()` | Tạo phiếu chi hoàn phí và cập nhật payment trong transaction. |
| `GenerateReceiptCodeLocked()` | Sinh mã phiếu thu/chi trong transaction. |
| `CountEventParticipants()` | Count người tham gia theo payment đã consume với event có phí. |

## Logic thu phí

1. User bấm thu phí cho participant.
2. Client gọi `load_payment_details`.
3. Server kiểm tra event có phí, còn hạn, participant đủ điều kiện với object.
4. Client submit `create_payment`.
5. `CreateEventPaymentAtomic()` trong transaction:
   - Lock kiểm tra chưa có phiếu thu active cho cùng event/participant.
   - Insert `EventPaymentConfirmation`.
   - Insert phiếu thu `ThuChi` với `loai='eventsukien'`.
   - Update `ThuChi.idhocvien` bằng participant id.

## Logic hoàn phí

1. User bấm hoàn phí.
2. Client submit `refund_payment`.
3. `RefundPaymentAtomic()` trong transaction:
   - Lock tìm phiếu thu active `PaymentStatus=1`.
   - Tạo phiếu chi `ThuChi` với `loai='eventsukien'`.
   - Update `EventPaymentConfirmation.PaymentStatus=3`.
   - Set `ConsumedAt=NULL`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Event` | Lấy thông tin phí, type, hạn đăng ký. |
| `EventPaymentConfirmation` | Trạng thái thu/hoàn phí event. |
| `ThuChi` | Phiếu thu/phiếu chi kế toán. |
| `EventRegistration`, `EventRegistrationByTeacher` | Registration được liên kết qua `ConsumedAt`. |
| `Hocsinh`, `Nhan_su` | Participant nhận phiếu. |

## Ý nghĩa trạng thái quan trọng

| Field | Ý nghĩa |
| --- | --- |
| `PaymentStatus=1` | Đã thu. |
| `PaymentStatus=3` | Đã hoàn. |
| `Enable=1` | Phiếu còn hiệu lực. |
| `ConsumedAt IS NULL` | Phiếu đã thu nhưng chưa được dùng cho đăng ký thật. |
| `ConsumedAt IS NOT NULL` | Phiếu đã được consume vào registration. |
| `ReceiptSource='ThuChi'` | Phiếu kế toán liên quan nằm ở bảng `ThuChi`. |

## Tương tác với module khác

- `ThuChi` nhận phiếu thu/chi loại `eventsukien`.
- `EventRegistration*` phụ thuộc `ConsumedAt` để biết phiếu thu đã được dùng cho đăng ký.
- `MODULE/Sukien` nếu sửa/xóa event ngoài flow này có thể làm payment/count lệch.

## Vấn đề cần sửa

- Giữ transaction và lock trong `CreateEventPaymentAtomic()`/`RefundPaymentAtomic()` khi sửa.
- Không tạo nhiều phiếu thu active cho cùng event/participant.
- Không hoàn phí nếu không có phiếu thu active.
- Cần làm rõ ý nghĩa `ThuChi.idhocvien` khi participant là giáo viên.
- Cần kiểm tra dữ liệu nếu `EventPaymentConfirmation` và `ThuChi` bị sửa lệch qua DB hoặc module khác.
