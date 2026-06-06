# HOCVIEN: Đăng ký học viên tham gia Event

## Mục đích

HOCVIEN có một điểm nối sang module Event để đăng ký học viên tham gia sự kiện từ màn danh sách học viên.

File liên quan:

- `MODULE/HOCVIEN/Danhsach.ascx`
- `MODULE/HOCVIEN/Danhsach.ascx.cs`
- `MODULE/Event/Manage.ascx.cs`
- `MODULE/Event/List.ascx.cs`
- `MODULE/Event/js/event.js`

## Trạng thái sử dụng

Trong `Danhsach.ascx.cs`, mỗi row học viên render action:

```html
btn-eventRegistration
```

Script trong `Danhsach.ascx` gọi:

```text
Default.aspx?mod=event!manage&cmd=load_register
Default.aspx?mod=event!manage&cmd=save
```

Đây là flow đang tồn tại trong UI HOCVIEN, nhưng không phải flow event đầy đủ nhất.

## Luồng xử lý

1. User mở danh sách học viên.
2. User bấm "Đăng ký tham gia event".
3. Client gửi id học viên tới `event!manage&cmd=load_register`.
4. `Manage.ascx.cs` tìm các event object phù hợp dựa trên:
   - `Event.Enable=1`
   - `Event.ExpireRegistration > GETDATE()`
   - `EventObject`
   - `EventTarget`
   - `EventDisplay`
   - tuổi học viên
   - khóa học hiện tại của học viên
   - chi nhánh học viên
5. User chọn event/object.
6. Client submit `event!manage&cmd=save`.
7. Server insert hoặc update `EventRegistration`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Event` | Event đang mở |
| `EventObject` | Đối tượng áp dụng |
| `EventTarget` | Rule tuổi/khóa học |
| `EventDisplay` | Rule chi nhánh/hiển thị |
| `Hocsinh` | Học viên được đăng ký |
| `Lophoc_join`, `Khoahoc` | Khóa học hiện tại của học viên |
| `EventRegistration` | Kết quả đăng ký |

## Overlap với flow Event chính

`MODULE/Event/List.ascx.cs` có flow đăng ký mới hơn:

- Hỗ trợ event học viên và giáo viên.
- Có modal chọn chi nhánh/object/participant.
- Có xử lý event có phí.
- Có `EventPaymentConfirmation`.
- Có `ConsumedAt`.
- Có transaction trong `SaveRegistrationAtomic()`.

Trong khi `MODULE/Event/Manage.ascx.cs`:

- Chỉ xử lý học viên.
- Insert/update trực tiếp `EventRegistration`.
- Không kiểm tra event có phí.
- Không xử lý `EventPaymentConfirmation`.
- Không set `ConsumedAt`.
- **Không dùng transaction**.

## Vấn đề cần sửa

- Nếu event có phí, đăng ký từ HOCVIEN có thể bypass flow thu phí của Event.
- Điều kiện hết hạn dùng `ExpireRegistration > GETDATE()`, có thể chặn ngay khi qua giờ 00:00 nếu cột chỉ lưu ngày.
- SQL injection do truy vấn SQL nối chuỗi trực tiếp từ id học viên/event.
- Không có guard rõ cho event type `teacher`.
- Không dùng chung logic eligibility với `MODULE/Event/List.ascx.cs`, dễ lệch rule giữa hai màn.

## Đề xuất

Ưu tiên đưa action từ HOCVIEN sang flow đăng ký chính của `MODULE/Event/List.ascx.cs`, hoặc refactor `Manage.ascx.cs` để gọi chung service với:

- `LoadRegistrationMeta()`
- `LoadEligibleParticipants()`
- `SaveManualRegistration()`
- `SaveRegistrationAtomic()`

Nếu chưa refactor được, cần ít nhất chặn đăng ký event có phí trong `Manage.ascx.cs`.
