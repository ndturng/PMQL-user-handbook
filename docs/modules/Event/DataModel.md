# Data model module Event

## Bảng chính

### `Event`

Lưu thông tin event.

| Cột | Ý nghĩa trong module |
| --- | --- |
| `Id` | Khóa chính event. |
| `Name` | Tên sự kiện. |
| `Description` | Thể lệ/mô tả, edit bằng CKEditor. |
| `Price` | Phí event; `0` là miễn phí, `> 0` là có phí. |
| `streakDay` | Streak/chuỗi ngày sự kiện. |
| `quantity` | Số lượng. |
| `ExpireRegistration` | Hạn đăng ký. |
| `FromDate` | Ngày bắt đầu. |
| `ToDate` | Ngày kết thúc. |
| `Enable` | Event còn hiệu lực hay đã xóa mềm. |
| `AllowCustom` | Hình thức học/D-R: nghe tính, nhìn tính, online. |
| `replay` | Cho phép replay. |
| `[Type]` | `student` hoặc `teacher`. |
| `Icon`, `DesktopBackground`, `MobileBackground` | Ảnh hiển thị. |

### `EventObject`

Mỗi event có thể có nhiều đối tượng áp dụng. Ví dụ: nhóm học viên theo tuổi, nhóm theo khóa học, hoặc nhóm giáo viên theo khóa tập huấn.

| Cột | Ý nghĩa |
| --- | --- |
| `Id` | Khóa chính object. |
| `EventId` | Event cha. |
| `Name` | Tên đối tượng áp dụng. |

### `EventTarget`

Định nghĩa object áp dụng cho nhóm nào.

| Cột | Ý nghĩa |
| --- | --- |
| `EventObjectId` | Object được cấu hình. |
| `TargetType` | `age` hoặc `course`. |
| `TargetList` | Danh sách tuổi khi target theo tuổi. |
| `SubCourse` | Danh sách khóa học/khóa tập huấn khi target theo course. |

### `EventDisplay`

Định nghĩa object hiển thị/áp dụng theo chi nhánh hoặc tất cả.

| Cột | Ý nghĩa |
| --- | --- |
| `EventObjectId` | Object được cấu hình. |
| `DisplayWith` | Kiểu hiển thị/áp dụng. |
| `Pattern` | Danh sách chi nhánh khi cần giới hạn theo chi nhánh. |

### `EventRegistration`

Lưu đăng ký event cho học viên.

| Cột | Ý nghĩa |
| --- | --- |
| `StudentId` | Học viên. |
| `EventId` | Event. |
| `EventObjectId` | Đối tượng event. |
| `CreatedBy`, `CreatedAt` | Người tạo/thời điểm. |
| `Status` | Trạng thái đăng ký. |

### `EventRegistrationByTeacher`

Lưu đăng ký event cho giáo viên.

| Cột | Ý nghĩa |
| --- | --- |
| `UserId` | Giáo viên/nhân sự. |
| `EventId` | Event. |
| `EventObjectId` | Đối tượng event. |
| `CreatedBy`, `CreatedAt` | Người tạo/thời điểm. |
| `Status` | Trạng thái đăng ký. |

### `EventPaymentConfirmation`

Lưu trạng thái thu/hoàn phí event.

| Cột | Ý nghĩa |
| --- | --- |
| `EventId` | Event. |
| `EventObjectId` | Đối tượng event. |
| `StudentId` / `TeacherId` | Người tham gia. |
| `IdChiNhanh` | Chi nhánh ghi nhận. |
| `PaymentStatus` | `1` đã thu, `3` đã hoàn; code cũng có nhánh đọc `0`. |
| `ReceiptSource` | Nguồn phiếu, ví dụ `ThuChi`. |
| `ReceiptCode` | Mã phiếu thu/chi liên quan. |
| `PaymentDate` | Ngày thu. |
| `Note` | Ghi chú. |
| `ConsumedAt` | Đã được consume vào đăng ký hay chưa. |
| `Enable` | Phiếu còn hiệu lực. |

## Bảng ngoài module được dùng

| Bảng | Vai trò |
| --- | --- |
| `ThuChi` | Phiếu thu/phiếu chi khi thu/hoàn phí event. |
| `Hocsinh` | Danh sách học viên đủ điều kiện. |
| `Nhan_su` | Danh sách giáo viên đủ điều kiện. |
| `ChiNhanh` | Lọc theo cơ sở. |
| `Lophoc_join`, `Khoahoc` | Khóa học hiện tại của học viên. |
| `Giaovien_Taphuan`, `KhoaTapHuan` | Khóa tập huấn của giáo viên. |

## Quan hệ dữ liệu chính

```text
Event
  ├─ EventObject
  │   ├─ EventTarget
  │   └─ EventDisplay
  ├─ EventRegistration -> Hocsinh
  ├─ EventRegistrationByTeacher -> Nhan_su
  └─ EventPaymentConfirmation -> ThuChi

Hocsinh -> Lophoc_join -> Khoahoc
Nhan_su -> Giaovien_Taphuan -> KhoaTapHuan
```

## Điểm đúng cần giữ

- Các flow thu phí, hoàn phí, đăng ký, hủy đăng ký đã có transaction và lock ở các điểm quan trọng.
- `ConsumedAt` đang là tín hiệu quan trọng để biết phiếu thu đã được dùng cho đăng ký thật hay chưa.
- Xóa event có phí bị chặn nếu còn phiếu thu active.
- Đổi phí event đã có payment bị chặn ở server, không chỉ ở client.

## Issue DB cần chú ý

- Các bảng object/target/display dùng CSV trong cột (`TargetList`, `SubCourse`, `Pattern`), khó enforce toàn vẹn dữ liệu và query hiệu quả.
- `EventObject` không nên bị xóa nếu còn registration/payment tham chiếu.
- `EventPaymentConfirmation` và `ThuChi` phải đồng bộ; nếu một bên bị sửa thủ công, trạng thái thu/hoàn phí sẽ lệch.
- Nên kiểm tra index cho các cột: `EventId`, `StudentId`, `TeacherId`, `EventObjectId`, `PaymentStatus`, `ConsumedAt`, `Enable`.
