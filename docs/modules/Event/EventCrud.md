# Event CRUD và cấu hình đối tượng

## Feature

Feature này quản lý danh sách sự kiện, thêm/sửa/xóa event và cấu hình đối tượng áp dụng cho event.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Entry chính: `Default.aspx?mod=event!list`
- UI chính: `MODULE/Event/List.ascx`
- Code chính: `MODULE/Event/List.ascx.cs`
- JS chính: `MODULE/Event/js/event.js`

## Function/command liên quan

| Command/function | Vai trò |
| --- | --- |
| `cmd=load` / `Load_list()` | Load danh sách event. |
| `cmd=save` / `Savechange()` | Thêm/sửa event. |
| `UpdateEventWithFeeTransition()` | Update event khi có rule đổi phí. |
| `cmd=edit` / `GetEventById()` | Lấy dữ liệu event để sửa. |
| `cmd=delete_event_check` / `CheckDeleteEvent()` | Kiểm tra trước khi xóa event. |
| `cmd=delete` / `delete()` | Xóa mềm event. |
| `cmd=addObject` / `addObject()` | Tạo object áp dụng mới. |
| `cmd=editEventObject` / `GetEventObjectById()` | Lấy dữ liệu object. |
| `cmd=saveEventObject` / `SavechangeEventForm()` | Lưu target/display/object. |
| `cmd=deleteEventObject` / `deleteEventObject()` | Xóa object. |
| `GetEventObjects()` | Load object + target + display. |

## Logic load danh sách

1. User vào `Default.aspx?mod=event!list`.
2. `List.ascx` render bảng, filter, modal và load `/module/event/js/event.js`.
3. JS gọi `Loadlist()`.
4. Server chạy `Load_list()`.
5. Query `Event WHERE Enable=1`, có filter `[Type]`.
6. Với từng event, server tính số lượng người tham gia:
   - Event miễn phí: count từ `EventRegistration` hoặc `EventRegistrationByTeacher`.
   - Event có phí: count từ `EventPaymentConfirmation` với `PaymentStatus=1`, `Enable=1`, `ConsumedAt IS NOT NULL`.
7. Server render HTML rows và rows object con.
8. Client decode HTML và đưa vào `#showlist`.

## Logic tạo/sửa event

1. User mở modal `#exampleModal`.
2. Form submit tới `cmd=save`.
3. `Savechange()` insert nếu `id == 0`, update nếu `id != 0`.
4. Khi update, code có rule bảo vệ fee-transition:
   - Free sang paid và đã có đăng ký: yêu cầu xác nhận, sau đó có thể hủy đăng ký cũ.
   - Paid sang free: block nếu còn đăng ký hoặc phiếu thu active.
   - Paid sang paid nhưng đổi phí: block nếu còn phiếu thu active.
5. Update chính chạy qua `UpdateEventWithFeeTransition()` trong transaction serializable.

## Logic cấu hình đối tượng

1. User bấm `+ Thêm đối tượng`.
2. Server tạo `EventObject` mặc định tên `Đối tượng mới`.
3. User sửa object trong modal.
4. `SavechangeEventForm()` upsert:
   - `EventTarget`: target theo tuổi hoặc khóa học/tập huấn.
   - `EventDisplay`: áp dụng theo chi nhánh hoặc rule hiển thị.
   - `EventObject.Name`.

Object là điều kiện bắt buộc để flow đăng ký mới tìm được người phù hợp.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Event` | Bảng event chính. |
| `EventObject` | Nhóm đối tượng áp dụng của event. |
| `EventTarget` | Điều kiện tuổi/khóa học/khóa tập huấn. |
| `EventDisplay` | Điều kiện chi nhánh/hiển thị. |
| `EventRegistration`, `EventRegistrationByTeacher` | Dùng để kiểm tra đăng ký khi đổi phí/xóa/count. |
| `EventPaymentConfirmation` | Dùng để kiểm tra phiếu thu active khi đổi phí/xóa/count. |

## Overlap/xung đột

- `MODULE/Sukien` cũng thao tác bảng `Event` nhưng không chạy đầy đủ rule của `MODULE/Event`.
- `Manage.ascx.cs` đăng ký học viên theo object nhưng không đi qua toàn bộ rule registration/payment mới.
- Xóa `EventObject` có thể ảnh hưởng registration/payment đang tham chiếu object đó.

## Vấn đề cần sửa

- Sửa lỗi SQL injection trong CRUD và object config bằng cách thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query.
- Không cho xóa `EventObject` nếu đã có registration/payment liên quan; hoặc chuyển sang soft delete nếu schema cho phép.
- Bọc `delete()` event vào transaction đầy đủ.
- Tách business logic khỏi `List.ascx.cs` vì file này đang gánh quá nhiều trách nhiệm.
