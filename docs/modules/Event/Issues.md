# Issues module Event

## Overlap/xung đột với module khác

### Với `MODULE/Sukien`

`MODULE/Sukien` là module sự kiện legacy nhưng vẫn thao tác trực tiếp bảng `Event`. Nó có thể tạo/sửa/xóa event mà không chạy các rule của `MODULE/Event`.

Rủi ro chính:

- Event tạo từ `Sukien` có thể thiếu `ExpireRegistration`, `[Type]`, `quantity`, `streakDay`, `AllowCustom`, `replay`.
- `Sukien` không tạo `EventObject`, nên event có thể hiện trong list mới nhưng không đăng ký được.
- `Sukien` có thể sửa `price` mà không kiểm tra đăng ký/phiếu thu.
- Cả hai dùng permission key `18SK`, nên user có quyền sự kiện có thể vào URL legacy nếu biết đường dẫn.

### Với `MODULE/HOCVIEN`

`MODULE/HOCVIEN/Danhsach.ascx` có flow đăng ký event cho học viên qua `event!manage`. Flow này dùng một logic đăng ký cũ hơn `SaveManualRegistration()` trong `List.ascx.cs`.

Rủi ro chính:

- `Manage.ascx.cs` chỉ xử lý học viên, không xử lý giáo viên.
- `Manage.ascx.cs` insert/update trực tiếp `EventRegistration`, không xử lý event có phí và không consume `EventPaymentConfirmation`.
- Điều kiện hết hạn trong `Manage.ascx.cs` dùng `e.ExpireRegistration > GETDATE()`, có thể khác rule ngày trong flow chính.

### Với kế toán/thu chi

Thu phí và hoàn phí trong `MODULE/Event` tạo bản ghi `ThuChi` với `loai='eventsukien'`, đồng thời ghi `EventPaymentConfirmation`.

Rủi ro chính:

- Nếu sửa/xóa event ngoài `MODULE/Event`, dữ liệu `ThuChi` không được đồng bộ.
- `ThuChi.idhocvien` được update bằng participant id cho cả học viên và giáo viên, nên ý nghĩa cột phụ thuộc ngữ cảnh event.
- Mã phiếu thu/chi được sinh bằng `GenerateReceiptCodeLocked()` và count theo chi nhánh; cần giữ transaction/lock khi sửa.

## Vấn đề DB và dữ liệu

- Nhiều đoạn vẫn nối SQL từ request: `GetEventById()`, `GetEventObjectById()`, `SavechangeEventForm()`, `Load_list()` một phần, `uploadImage.ascx.cs`, `Manage.ascx.cs`, `list-registration.ascx.cs`.
- `SafeSql()` chỉ replace dấu nháy, không xử lý triệt để SQL injection; cần dùng parameterized query.
- `deleteEventObject()` xóa thẳng `EventObject` nhưng không kiểm tra registration/payment đang tham chiếu object đó.
- `delete()` soft-delete `Event` trước rồi xóa đăng ký dạng best-effort, không gom toàn bộ vào transaction.
- `uploadImage.ascx.cs` cho phép lưu extension từ file upload, chưa thấy validate MIME/extension/size.
- `Manage.ascx.cs` là flow đăng ký cũ, không đồng bộ rule paid-event/`ConsumedAt` với flow chính.
- `list-registration.ascx.cs` có command `cancel` cũ update `Lophoc_join`, không đúng domain event registration và có vẻ không còn khớp với nút hủy hiện tại.
- Event có phí phụ thuộc `EventPaymentConfirmation` và `ThuChi`; nếu sửa DB thủ công hoặc qua module legacy, count/trạng thái dễ lệch.
- Các bảng object/target/display dùng CSV trong cột (`TargetList`, `SubCourse`, `Pattern`), khó enforce toàn vẹn dữ liệu và query hiệu quả.

## Vấn đề logic, bảo mật, hiệu suất cần sửa

### Ưu tiên cao

- Chặn hoặc redirect `MODULE/Sukien` để không bypass rule của module này.
- Đưa `Manage.ascx.cs` về dùng chung service/logic với `SaveManualRegistration()` hoặc vô hiệu flow cũ nếu không còn dùng.
- Sửa lỗi SQL injection: thay các truy vấn SQL nối chuỗi trực tiếp bằng parameterized query, đặc biệt các truy vấn nhận `id`, keyword, object config, upload field.
- Không cho xóa `EventObject` nếu đã có registration/payment liên quan; hoặc soft-delete object nếu schema cho phép.
- Bọc `delete()` event vào transaction đầy đủ: disable event, xóa registration, cập nhật payment liên quan nếu cần.
- Validate upload file: extension whitelist, MIME, size, tên file, và quyền user.

### Ưu tiên trung bình

- Chuẩn hóa rule hết hạn đăng ký theo ngày: nếu nghiệp vụ là hết hạn cuối ngày, tránh dùng `ExpireRegistration < DateTime.Now` hoặc `> GETDATE()` theo giờ.
- Chuẩn hóa JSON response: nơi dùng `JsonResponse()`, nơi nối chuỗi thủ công.
- Tách business logic event/payment/registration khỏi code-behind để tránh một file `List.ascx.cs` quá lớn.
- Thêm filter chi nhánh rõ ở list event nếu dữ liệu event cần phân quyền theo cơ sở.
- Thêm index hoặc kiểm tra index cho các cột: `EventId`, `StudentId`, `TeacherId`, `EventObjectId`, `PaymentStatus`, `ConsumedAt`, `Enable`.

### Ưu tiên thấp

- Xóa hoặc khóa các debug endpoint như `DebugGetCourses()`/`DebugEventObject()` nếu không dùng.
- Chuẩn hóa tiếng Việt trong response; hiện có nhiều message không dấu.
- Chuẩn hóa tên cột/case trong code: `Id/id`, `Enable/enable`, `[Type]`.

## Khi cần sửa/thêm tính năng

Nếu sửa event CRUD:

1. Kiểm tra `Savechange()` và `UpdateEventWithFeeTransition()`.
2. Đảm bảo không bypass rule đổi phí.
3. Kiểm tra tác động tới `EventObject`, registration, payment, `ThuChi`.
4. Nếu thêm field mới, update cả `GetEventById()`, form `List.ascx`, submit JS và `Savechange()`.

Nếu sửa đăng ký:

1. Kiểm tra cả `SaveManualRegistration()` và `Manage.ascx.cs`.
2. Xác định event type là `student` hay `teacher`.
3. Với event có phí, giữ nguyên rule `PaymentStatus=1` và `ConsumedAt IS NULL` trước khi đăng ký.
4. Sau đăng ký thành công, payment phải được set `ConsumedAt`.
5. Khi hủy đăng ký, payment phải được unconsume nếu event có phí.

Nếu sửa thu/hoàn phí:

1. Giữ transaction và lock trong `CreateEventPaymentAtomic()`/`RefundPaymentAtomic()`.
2. Kiểm tra `EventPaymentConfirmation` và `ThuChi` luôn đồng bộ.
3. Không tạo nhiều phiếu thu active cho cùng event/participant.
4. Không hoàn phí nếu không có phiếu thu active.

Nếu sửa object/target/display:

1. Kiểm tra các hàm eligibility: `IsStudentEligibleForObject()`, `IsTeacherEligibleForObject()`, `IsBranchEligible()`.
2. Không xóa object đang có registration/payment.
3. Kiểm tra cả flow đăng ký trong Event list và flow từ HOCVIEN.

Nếu sửa upload ảnh:

1. Kiểm tra quyền người dùng trước khi upload/xóa.
2. Validate file.
3. Đảm bảo xóa file cũ không làm mất ảnh đang được dùng bởi event khác.
4. Lưu URL/path nhất quán với môi trường remote server.
