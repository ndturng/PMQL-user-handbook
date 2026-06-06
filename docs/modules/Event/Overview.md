# Module Event (`MODULE/Event`)

## Mục đích

`MODULE/Event` là module quản lý sự kiện chính hiện tại của app. Module này bao gồm:

- Tạo, sửa, xóa mềm sự kiện.
- Phân loại sự kiện cho học viên hoặc giáo viên.
- Cấu hình đối tượng áp dụng cho từng sự kiện.
- Lọc người đủ điều kiện tham gia theo chi nhánh, tuổi, khóa học hoặc khóa tập huấn.
- Đăng ký/hủy đăng ký người tham gia.
- Thu phí sự kiện, hoàn phí và liên kết với `ThuChi`.
- Upload ảnh sự kiện: icon, background desktop, background mobile.
- Xem danh sách người đã đăng ký trong popup iframe.

Entry chính:

```text
Default.aspx?mod=event!list
```

## Trạng thái sử dụng trong app

Module này đang là flow chính của menu Sự kiện.

Bằng chứng trong code:

- `MODULE/menu/menu_left.ascx` trỏ menu Sự kiện tới `Default.aspx?mod=event!list`.
- `MODULE/HOCVIEN/Danhsach.ascx` mở flow đăng ký event qua `Default.aspx?mod=event!manage`.
- `MODULE/Event/List.ascx` load script `/module/event/js/event.js` và mở danh sách đăng ký qua `Frame-ajax.aspx?mod=event!list-registration`.

Kết luận thực tế: mọi thay đổi vào module này có rủi ro cao hơn `MODULE/Sukien`, vì nó chạm trực tiếp vào flow quản lý sự kiện, đăng ký, thu phí và dữ liệu kế toán.

## Tài liệu con

- [EventCrud.md](./EventCrud.md): danh sách sự kiện, thêm/sửa/xóa event, cấu hình object/target/display.
- [Registration.md](./Registration.md): lọc người đủ điều kiện, đăng ký/hủy đăng ký, popup danh sách đăng ký.
- [Payment.md](./Payment.md): thu phí, hoàn phí, liên kết `EventPaymentConfirmation` với `ThuChi`.
- [MediaUpload.md](./MediaUpload.md): upload/xóa ảnh sự kiện.
- [DataModel.md](./DataModel.md): bảng dữ liệu, quan hệ và flow dữ liệu chính.
- [Issues.md](./Issues.md): overlap, rủi ro logic, DB, bảo mật, hiệu suất và việc cần sửa.

## File liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/Event/List.ascx` | UI chính: bảng sự kiện, modal event, modal object, modal đăng ký, modal thu/hoàn phí. |
| `MODULE/Event/List.ascx.cs` | Code-behind chính: CRUD event, object, đăng ký, thu phí, hoàn phí, count, rule fee-transition. |
| `MODULE/Event/js/event.js` | Client-side logic cho list, modal, đăng ký, thu phí, hoàn phí, upload. |
| `MODULE/Event/Manage.ascx` | Control nhẹ cho flow đăng ký từ màn học viên. |
| `MODULE/Event/Manage.ascx.cs` | Đăng ký học viên vào event/object từ `MODULE/HOCVIEN`. |
| `MODULE/Event/list-registration.ascx` | UI popup danh sách người tham gia. |
| `MODULE/Event/list-registration.ascx.cs` | Load danh sách đăng ký học viên/giáo viên theo event. |
| `MODULE/Event/uploadImage.ascx` | UI upload/xóa ảnh sự kiện. |
| `MODULE/Event/uploadImage.ascx.cs` | Lưu file vào `/uploads`, update ảnh trong bảng `Event`. |
| `MODULE/HOCVIEN/Danhsach.ascx` | Có nút/flow đăng ký học viên tham gia event qua `event!manage`. |
| `MODULE/Sukien/Danhsach.ascx.cs` | Module legacy dùng chung bảng `Event`, có thể gây overlap dữ liệu. |

## Feature map

| Feature | Entry/command | File chính | Bảng chính |
| --- | --- | --- | --- |
| Danh sách sự kiện | `cmd=load` | `List.ascx.cs: Load_list()` | `Event`, `EventObject`, registration/payment tables |
| Thêm/sửa sự kiện | `cmd=save`, `cmd=edit` | `Savechange()`, `GetEventById()` | `Event` |
| Xóa sự kiện | `cmd=delete_event_check`, `cmd=delete` | `CheckDeleteEvent()`, `delete()` | `Event`, `EventRegistration*`, `EventPaymentConfirmation` |
| Cấu hình đối tượng | `cmd=addObject`, `cmd=editEventObject`, `cmd=saveEventObject`, `cmd=deleteEventObject` | `addObject()`, `GetEventObjectById()`, `SavechangeEventForm()` | `EventObject`, `EventTarget`, `EventDisplay` |
| Tải người đủ điều kiện | `cmd=registration_meta`, `cmd=registration_participants` | `LoadRegistrationMeta()`, `LoadEligibleParticipants()` | `Hocsinh`, `Nhan_su`, `Lophoc_join`, `Giaovien_Taphuan`, object tables |
| Đăng ký thủ công | `cmd=save_registration` | `SaveManualRegistration()`, `SaveRegistrationAtomic()` | `EventRegistration`, `EventRegistrationByTeacher`, `EventPaymentConfirmation` |
| Hủy đăng ký | `cmd=cancel_registration` | `CancelRegistration()`, `CancelRegistrationAtomic()` | `EventRegistration*`, `EventPaymentConfirmation` |
| Thu phí | `cmd=create_payment` | `CreateEventPayment()`, `CreateEventPaymentAtomic()` | `EventPaymentConfirmation`, `ThuChi` |
| Hoàn phí | `cmd=refund_payment` | `RefundPayment()`, `RefundPaymentAtomic()` | `EventPaymentConfirmation`, `ThuChi` |
| Danh sách đăng ký popup | `mod=event!list-registration` | `list-registration.ascx.cs` | `EventRegistration*`, `Hocsinh`, `Nhan_su` |
| Đăng ký từ hồ sơ học viên | `mod=event!manage` | `Manage.ascx.cs` | `EventObject`, `EventTarget`, `EventDisplay`, `EventRegistration` |
| Upload ảnh | `mod=event!uploadImage` | `uploadImage.ascx.cs` | `Event`, filesystem `/uploads` |

## Các module/feature liên quan

| Module/feature | Tương tác |
| --- | --- |
| `MODULE/HOCVIEN` | Mở đăng ký event cho học viên qua `event!manage`; bị ảnh hưởng bởi event/object/target/display. |
| `MODULE/Sukien` | Legacy CRUD cùng bảng `Event`; có thể phá rule hoặc tạo dữ liệu thiếu. |
| Kế toán/thu chi | Nhận phiếu thu/chi `ThuChi` loại `eventsukien` khi thu/hoàn phí. |
| Quản lý học viên/lớp | Dùng `Hocsinh`, `Lophoc_join`, `Khoahoc` để lọc học viên phù hợp. |
| Nhân sự/giáo viên | Dùng `Nhan_su`, `Giaovien_Taphuan`, `KhoaTapHuan` để lọc giáo viên phù hợp. |
| Chi nhánh | Lọc danh sách chi nhánh trong registration modal và list-registration popup. |
| File upload | Ghi file ảnh vào `/uploads` và lưu URL/path vào `Event`. |

## Nhận xét nhanh

`MODULE/Event` nhỏ về số lượng file nhưng lớn về nghiệp vụ. `List.ascx.cs` đang chứa nhiều trách nhiệm: CRUD, rule đổi phí, registration, payment, refund, count và object config. Khi sửa module này cần kiểm tra cả luồng đăng ký từ `HOCVIEN`, module legacy `Sukien` và dữ liệu `ThuChi`.
