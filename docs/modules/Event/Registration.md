# Event registration

## Feature

Feature này lọc người đủ điều kiện tham gia event, đăng ký/hủy đăng ký người tham gia và hiển thị danh sách đã đăng ký.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Đăng ký từ màn Event: `Default.aspx?mod=event!list`
- Đăng ký từ hồ sơ học viên: `Default.aspx?mod=event!manage`
- Popup danh sách đăng ký: `Frame-ajax.aspx?mod=event!list-registration&eventId={id}`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/Event/List.ascx.cs` | Registration flow chính trong màn Event. |
| `MODULE/Event/js/event.js` | Client-side load metadata, participants, submit registration/cancel. |
| `MODULE/Event/Manage.ascx.cs` | Flow đăng ký học viên từ `HOCVIEN`, có logic cũ hơn. |
| `MODULE/Event/list-registration.ascx.cs` | Load popup danh sách người tham gia. |
| `MODULE/HOCVIEN/Danhsach.ascx` | Mở flow đăng ký event cho học viên. |

## Function/command liên quan

| Command/function | Vai trò |
| --- | --- |
| `cmd=registration_meta` / `LoadRegistrationMeta()` | Lấy metadata cho modal đăng ký. |
| `cmd=registration_participants` / `LoadEligibleParticipants()` | Lọc học viên/giáo viên đủ điều kiện. |
| `cmd=save_registration` / `SaveManualRegistration()` | Lưu đăng ký thủ công. |
| `SaveRegistrationAtomic()` | Insert/update registration và consume payment nếu event có phí. |
| `cmd=cancel_registration` / `CancelRegistration()` | Hủy đăng ký single/bulk. |
| `CancelRegistrationAtomic()` | Xóa registration và unconsume payment. |
| `list-registration.ascx.cs` | Load danh sách người đã đăng ký trong iframe. |

## Logic đăng ký từ màn Event

1. User bấm `Đăng ký`.
2. Client gọi `registration_meta` để lấy event, type, phí, chi nhánh, object.
3. User chọn chi nhánh/object và tải participants qua `registration_participants`.
4. Server lọc:
   - `student`: từ `Hocsinh`, tuổi, khóa học đang học.
   - `teacher`: từ `Nhan_su`, khóa tập huấn.
5. User chọn participants và submit `save_registration`.
6. Nếu event có phí, từng participant phải có `EventPaymentConfirmation` đã thu và `ConsumedAt IS NULL`.
7. `SaveRegistrationAtomic()` insert/update registration trong transaction, sau đó set `ConsumedAt=GETDATE()` cho payment.

## Logic hủy đăng ký

1. User hủy một hoặc nhiều participant.
2. Server chạy `CancelRegistration()`.
3. `CancelRegistrationAtomic()` xóa registration trong transaction.
4. Nếu event có phí, set `EventPaymentConfirmation.ConsumedAt=NULL` để phiếu thu không còn được tính là đã consume vào đăng ký.

## Logic popup danh sách đăng ký

`list_registration(eventId)` mở colorbox iframe:

```text
Frame-ajax.aspx?mod=event!list-registration&eventId={id}
```

`list-registration.ascx.cs` đọc `Event.[Type]` để chọn bảng:

- `student`: `EventRegistration`.
- `teacher`: `EventRegistrationByTeacher`.

Với chi nhánh khác `1`, danh sách bị lọc theo chi nhánh của học viên/giáo viên.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Event` | Xác định event, type, hạn đăng ký, phí. |
| `EventObject` | Object được chọn khi đăng ký. |
| `EventTarget` | Rule tuổi/khóa học/khóa tập huấn. |
| `EventDisplay` | Rule chi nhánh. |
| `EventRegistration` | Đăng ký học viên. |
| `EventRegistrationByTeacher` | Đăng ký giáo viên. |
| `EventPaymentConfirmation` | Phiếu thu cần consume khi event có phí. |
| `Hocsinh`, `Lophoc_join`, `Khoahoc` | Lọc học viên đủ điều kiện. |
| `Nhan_su`, `Giaovien_Taphuan`, `KhoaTapHuan` | Lọc giáo viên đủ điều kiện. |

## Overlap/xung đột

- `Manage.ascx.cs` là flow đăng ký cũ từ `HOCVIEN`, chỉ xử lý học viên và không đồng bộ đầy đủ rule paid-event/`ConsumedAt`.
- `list-registration.ascx.cs` có command `cancel` cũ update `Lophoc_join`, không đúng domain event registration và có vẻ không còn khớp với nút hủy hiện tại.
- Event type `student`/`teacher` quyết định bảng registration khác nhau, nên mọi thay đổi cần xử lý cả hai nhánh.

## Vấn đề cần sửa

- Đưa `Manage.ascx.cs` về dùng chung service/logic với `SaveManualRegistration()` hoặc vô hiệu flow cũ nếu không còn dùng.
- Chuẩn hóa rule hết hạn đăng ký theo ngày/giờ giữa các flow.
- Sửa lỗi SQL injection trong `Manage.ascx.cs` và `list-registration.ascx.cs` bằng parameterized query.
- Kiểm tra kỹ `ConsumedAt`: event có phí chỉ được count là registered khi payment đã được consume.
