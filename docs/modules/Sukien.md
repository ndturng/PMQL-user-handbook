# Module: Sự kiện legacy (`MODULE/Sukien`)

## Mục đích

`MODULE/Sukien` là module quản lý danh sách sự kiện cũ. Module này cho phép người dùng:

- Xem danh sách sự kiện theo mã sự kiện.
- Tìm kiếm sự kiện theo tên hoặc mã.
- Thêm sự kiện mới.
- Sửa thông tin sự kiện.
- Xóa mềm sự kiện bằng cách set `Event.Enable = 0`.

Module được load qua URL dạng:

```text
Default.aspx?mod=sukien!Danhsach
```

## Trạng thái sử dụng trong app

Module này **không có dấu hiệu là flow chính hiện tại của menu Sự kiện**.

Bằng chứng trong code:

- `MODULE/menu/menu_left.ascx` đang trỏ menu Sự kiện tới `Default.aspx?mod=event!list`.
- Không thấy menu chính trỏ tới `Default.aspx?mod=sukien!Danhsach`.
- `MODULE/Event` đang là module sự kiện đầy đủ hơn, có quản lý đối tượng sự kiện, đăng ký, thu phí, hoàn phí, upload ảnh và danh sách đăng ký.

Kết luận thực tế: `MODULE/Sukien` nhiều khả năng là module cũ/legacy hoặc màn hình còn truy cập được bằng URL trực tiếp. Không nên coi đây là nguồn nghiệp vụ chính cho sự kiện nếu chưa xác nhận với người vận hành.

## Module/feature overlap hoặc xung đột

`MODULE/Sukien` overlap trực tiếp với `MODULE/Event` vì cả hai cùng thao tác bảng `Event` và cùng dùng permission key `18SK`.

| Khu vực | `MODULE/Sukien` | `MODULE/Event` | Rủi ro |
| --- | --- | --- | --- |
| Entry URL | `mod=sukien!Danhsach` | `mod=event!list` | Người dùng có thể sửa cùng dữ liệu từ 2 màn hình khác logic |
| Bảng chính | `Event` | `Event` | Dữ liệu tạo từ module này có thể thiếu cột mà module kia cần |
| Permission | `18SK` / `18SK ` | `18SK` | Cùng quyền nhưng behavior khác nhau |
| Quản lý object | Không có | Có `EventObject`, `EventTarget`, `EventDisplay` | Event tạo từ `Sukien` có thể không có object nên không đủ điều kiện cho flow đăng ký mới |
| Đăng ký/thu phí | Không có | Có đăng ký học viên/giáo viên, thu phí, hoàn phí | Sửa phí/ngày bằng `Sukien` có thể bypass rule bảo vệ của `MODULE/Event` |
| Upload ảnh | Chỉ có `imageUrl` text | Có upload `Icon`, `DesktopBackground`, `MobileBackground` | Dữ liệu ảnh không đồng nhất giữa 2 màn hình |

Điểm xung đột quan trọng nhất: `MODULE/Event` có logic bảo vệ khi đổi phí event đã có đăng ký/phiếu thu, còn `MODULE/Sukien` update `price` trực tiếp, không kiểm tra đăng ký, không kiểm tra thanh toán và không chạy transaction.

## File liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/Sukien/Danhsach.ascx` | UI danh sách, form modal thêm/sửa, JavaScript AJAX |
| `MODULE/Sukien/Danhsach.ascx.cs` | Code-behind xử lý quyền, load chi nhánh, CRUD và trả JSON |
| `MODULE/Event/List.ascx.cs` | Module sự kiện chính hiện tại, dùng chung bảng `Event` nhưng có logic nghiệp vụ đầy đủ hơn |
| `MODULE/Event/Manage.ascx.cs` | Đăng ký học viên vào event/object |
| `MODULE/Event/list-registration.ascx.cs` | Danh sách đăng ký event |
| `MODULE/Event/uploadImage.ascx.cs` | Upload ảnh event trong flow mới |
| `MODULE/HOCVIEN/Danhsach.ascx` | Có UI đăng ký học viên tham gia event qua `event!manage` |
| `MODULE/menu/menu_left.ascx` | Menu chính trỏ tới `event!list`, không trỏ tới `sukien!Danhsach` |
| `App_Code/BO/connect.cs` | Helper truy vấn database: `showall`, `inserreturn`, `update` |
| `App_Code/BO/users.cs` | Kiểm tra quyền module và lấy chi nhánh user |
| `App_Code/BO/ChinhNhanh.cs` | Load danh sách chi nhánh user được phép xem |

## Bảng dữ liệu chính

### `Event`

Module này thao tác trực tiếp trên bảng `Event`.

Những cột được dùng rõ trong code `MODULE/Sukien`:

| Cột | Ý nghĩa trong module |
| --- | --- |
| `id` | Khóa chính sự kiện |
| `code` / `Code` | Mã sự kiện, dùng để nhóm danh sách |
| `name` | Tên sự kiện |
| `description` | Mô tả ngắn |
| `price` | Phí sự kiện |
| `fromdate` | Ngày bắt đầu |
| `todate` | Ngày kết thúc, dùng để tính trạng thái hết hạn |
| `DateCreate` | Ngày tạo |
| `userId` | User tạo sự kiện |
| `idchinhanh` | Chi nhánh tạo/sở hữu sự kiện |
| `enable` / `Enable` | Có xóa mềm hay không |
| `active` | Có đang hoạt động hay không |
| `ispublic` | Có public hay không khi tạo mới |
| `imageUrl` | Ảnh đại diện/URL ảnh |

## DB và vấn đề dữ liệu

### Vấn đề trên bảng `Event`

`MODULE/Sukien` chỉ insert/update một tập cột nhỏ của bảng `Event`, trong khi `MODULE/Event` đang dùng thêm nhiều cột khác như:

- `streakDay`
- `quantity`
- `ExpireRegistration`
- `AllowCustom`
- `replay`
- `[Type]`
- `CreatedBy`
- `CreatedAt`
- `Icon`
- `DesktopBackground`
- `MobileBackground`

Rủi ro: event được tạo từ `MODULE/Sukien` có thể thiếu giá trị cho các cột mà flow mới cần. Nếu DB không có default hợp lý, các màn hình bên `MODULE/Event`, `MODULE/Event/Manage.ascx.cs` hoặc danh sách đăng ký có thể hiển thị sai, lọc sai hoặc lỗi parse ngày/null.

### Vấn đề toàn vẹn dữ liệu

- `Sukien` không tạo `EventObject`, nên event mới có thể không có đối tượng tham gia.
- `Sukien` không kiểm tra `EventRegistration`, `EventRegistrationByTeacher`, `EventPaymentConfirmation` trước khi sửa/xóa event.
- `Sukien` xóa mềm `Event.Enable = 0` nhưng không xử lý đăng ký, payment confirmation hoặc các bảng con.
- `Sukien` nhận `chinhanh` từ client nhưng `Load_list()` không lọc theo `idchinhanh`, nên danh sách có thể lộ dữ liệu liên chi nhánh nếu user truy cập được màn này.
- Không thấy ràng buộc unique trong code cho `Event.Code`; danh sách còn nhóm theo `Code`, nên trùng code có thể làm UI gom sai nhóm.

### Bảng liên quan bị ảnh hưởng gián tiếp

| Bảng | Cách bị ảnh hưởng |
| --- | --- |
| `EventObject` | Event tạo từ `Sukien` không tạo object; flow đăng ký theo object có thể không dùng được |
| `EventTarget` | Không có target cho event/object nên không map được nhóm học viên/giáo viên phù hợp |
| `EventDisplay` | Không có rule hiển thị/lộ trình cho object |
| `EventRegistration` | Nếu đã có đăng ký, `Sukien` vẫn cho sửa/xóa event mà không xử lý trạng thái đăng ký |
| `EventRegistrationByTeacher` | Tương tự `EventRegistration`, nhưng cho event giáo viên |
| `EventPaymentConfirmation` | Sửa phí/xóa event không kiểm tra phiếu thu/hoàn phí |
| `ThuChi` | `MODULE/Event` tạo phiếu thu/chi loại `eventsukien`; `Sukien` không đồng bộ gì với các phiếu này |

## Phân quyền

Code-behind kiểm tra quyền trong `Page_Load`:

```csharp
Users.Check_PerBymod("18SK ", 1)
```

Nếu không có quyền xem, user bị redirect:

```text
/default.aspx?mod=erros!permistion
```

Nút thao tác trên UI dùng CSS permission:

| Hành động | Mã quyền |
| --- | --- |
| Thêm mới | `Users.Check_PerBymod_Css("18SK", 2)` |
| Sửa | `Users.Check_PerBymod_Css("18SK", 3)` |
| Xóa | `Users.Check_PerBymod_Css("18SK", 4)` |

Lưu ý: permission check trong `Page_Load` đang dùng chuỗi `"18SK "` có dấu cách cuối, trong khi các nút dùng `"18SK"`. Nếu sửa module này, cần kiểm tra xem dấu cách đó là typo hay đang phụ thuộc vào dữ liệu quyền hiện tại.

## Luồng xử lý chính

### 1. Load màn hình

1. User vào `Default.aspx?mod=sukien!Danhsach`.
2. `Default.aspx` load control `MODULE/Sukien/Danhsach.ascx`.
3. `Page_Load` của `Danhsach.ascx.cs` kiểm tra quyền xem.
4. `loadchinhanh()` nạp danh sách chi nhánh vào select `#chinhanh`.
5. JavaScript trong `.ascx` gọi `Loadlist()` sau 1 giây.
6. `Loadlist()` gọi AJAX:

```text
Default.aspx?mod=sukien!Danhsach&cmd=load&searchString=...&chinhanh=...
```

7. Server chạy `Load_list()` và trả JSON gồm HTML table row đã encode base64/json.
8. Client decode bằng `decode_base(data.str)` rồi gán vào `#showlist`.

### 2. Tìm kiếm

Input `#searchString` có debounce 700ms.

Mỗi lần user nhập, client gọi lại `Loadlist()`.

Server tạo điều kiện:

```sql
and (name like N'{searchString}' or code ='{searchString}')
```

Lưu ý logic hiện tại không thêm `%` cho `LIKE`, nên `name like N'abc'` gần như tìm chính xác chuỗi `abc`, không phải contains search.

### 3. Thêm mới sự kiện

1. User bấm `#btn-add`.
2. Client set `#id = 0`, clear các input `.form-control`, mở modal `#exampleModal`.
3. Submit form `#eventForm`.
4. Client gửi `FormData` tới:

```text
/default.aspx?mod=sukien!Danhsach&cmd=save
```

5. Server chạy `Savechange()`.
6. Nếu `Request.Form["id"] == "0"`, server insert vào `Event`.
7. Insert thành công thì trả:

```json
{"success":true,"message":"Thành công: Đã thêm sự kiện thành công!"}
```

8. Client gọi `Loadlist()`, đóng modal, hiện `showSuccess`.

Giá trị mặc định khi insert:

| Cột | Giá trị |
| --- | --- |
| `DateCreate` | `getdate()` |
| `userId` | `checkusers.iduser()` |
| `idchinhanh` | `Users.Get_ChiNhanhbyID(...)` |
| `enable` | `1` |
| `active` | `1` |
| `ispublic` | `1` |

### 4. Sửa sự kiện

1. User bấm nút `.btn-edit`.
2. Client lấy `data-id`.
3. Client gọi AJAX:

```text
Default.aspx?mod=sukien!Danhsach&cmd=edit
```

với form field `id`.

4. Server chạy `GetEventById()`, query:

```sql
select * from Event where id = {id}
```

5. Server trả JSON các field: `code`, `name`, `price`, `description`, `fromdate`, `todate`, `imageUrl`.
6. Client fill modal và show form.
7. Khi submit, `Savechange()` update bản ghi nếu `id != "0"`.

Update hiện tại chỉ cập nhật:

```text
code, name, price, description, fromdate, todate
```

Lưu ý: `imageUrl` được load vào modal khi edit và được insert khi thêm mới, nhưng không được update trong nhánh sửa.

### 5. Xóa sự kiện

1. User bấm `.btn-delete`.
2. Client confirm bằng `confirm("Bạn có muốn xóa sự kiện này?")`.
3. Client gọi:

```text
Default.aspx?mod=sukien!Danhsach&cmd=delete
```

4. Server chạy `delete()`.
5. Server xóa mềm:

```sql
Update event set enable='0' where id='{id}'
```

6. Client reload danh sách bằng `Loadlist()`.

## Function map

| Function | File | Mục đích |
| --- | --- | --- |
| `Page_Load` | `Danhsach.ascx.cs` | Điều phối command `load`, `edit`, `save`, `delete`; kiểm tra quyền; load chi nhánh |
| `loadchinhanh()` | `Danhsach.ascx.cs` | Lấy danh sách chi nhánh user được xem và bind vào select |
| `Load_list()` | `Danhsach.ascx.cs` | Render HTML rows cho danh sách sự kiện và trả JSON |
| `GetEventById()` | `Danhsach.ascx.cs` | Lấy dữ liệu 1 sự kiện để fill modal sửa |
| `Savechange()` | `Danhsach.ascx.cs` | Insert hoặc update sự kiện |
| `delete()` | `Danhsach.ascx.cs` | Xóa mềm sự kiện |
| `Loadlist()` | `Danhsach.ascx` | Client-side AJAX load danh sách |
| `handleFormSubmit()` | `Danhsach.ascx` | Chặn double submit và gửi form AJAX |

## Trạng thái sự kiện trên danh sách

`Load_list()` tính trạng thái theo `todate` và `active`:

| Điều kiện | Trạng thái hiển thị |
| --- | --- |
| `todate > DateTime.Now` và `active == False` | Đã ngừng hoạt động |
| `todate > DateTime.Now` và `active != False` | Đang hoạt động |
| `todate <= DateTime.Now` | Đã kết thúc |

Lưu ý: so sánh dùng `DateTime.Now`, nên nếu `todate` chỉ là ngày không có giờ, sự kiện có thể bị xem là kết thúc ngay khi qua 00:00 của ngày đó.

## Tương tác với module/feature khác

### `MODULE/Event`

Đây là module bị ảnh hưởng nhiều nhất vì dùng chung bảng `Event`.

Các thay đổi từ `Sukien` có thể xuất hiện trong danh sách `MODULE/Event/List.ascx.cs` vì module mới query `Event WHERE enable=1`. Tuy nhiên, event tạo từ `Sukien` có thể thiếu `ExpireRegistration`, `[Type]`, `quantity`, `streakDay`, `AllowCustom`, `replay` hoặc object liên quan, nên khi sang module mới có thể phát sinh dữ liệu không đủ cấu hình.

Các rule quan trọng trong `MODULE/Event` mà `Sukien` không có:

- Không đổi event có phí thành miễn phí khi còn đăng ký hoặc phiếu thu chưa hoàn.
- Không đổi phí event có phí khi còn phiếu thu active.
- Khi chuyển free sang paid, có flow xác nhận và có thể hủy đăng ký cũ.
- Count người tham gia phụ thuộc event miễn phí/có phí và trạng thái `ConsumedAt`.
- Đăng ký học viên/giáo viên phụ thuộc `EventObject`, `EventTarget`, `EventDisplay`.

### `MODULE/HOCVIEN`

`MODULE/HOCVIEN/Danhsach.ascx` có flow đăng ký tham gia event qua `Default.aspx?mod=event!manage`, không gọi `MODULE/Sukien`. Data event được tạo/sửa từ `Sukien` vẫn có thể ảnh hưởng nếu nó xuất hiện trong danh sách event đủ điều kiện của `event!manage`.

### Kế toán/thu chi

`MODULE/Event` có logic tạo phiếu thu/phiếu chi trong `ThuChi` với loại `eventsukien`. `Sukien` không tạo, không hoàn, không vô hiệu hóa các phiếu này. Vì vậy sửa phí hoặc xóa event từ `Sukien` có thể làm lệch nghĩa giữa event và dữ liệu thu/chi đã ghi nhận.

### Menu và điều hướng

Menu trái hiện trỏ tới `event!list`, nên người dùng bình thường nhiều khả năng không đi qua `Sukien`. Nhưng vì module vẫn tồn tại và permission dùng chung `18SK`, user biết URL vẫn có thể truy cập nếu permission pass.

## Response format

Module này trả JSON thủ công bằng string concat.

Danh sách:

```json
{"status":"ok","str":"<base64/html rows>"}
```

Thêm/sửa/xóa:

```json
{"success":true,"message":"..."}
```

## Điểm cần cẩn thận khi sửa

- SQL injection do truy vấn SQL nối chuỗi trực tiếp từ `Request.Form` và `Request.QueryString`; cần thay bằng parameterized query.
- JSON cũng được nối chuỗi trực tiếp; giá trị có dấu nháy hoặc ký tự đặc biệt có thể làm hỏng response.
- `Load_list()` nhận `chinhanh` từ query/client nhưng hiện tại không lọc theo `idchinhanh`.
- `Load_list()` query danh sách `Code` trước, sau đó query lại từng code; nếu dữ liệu lớn sẽ tạo nhiều query.
- `delete()` lấy `idchinhanh` nhưng không dùng.
- Thêm mới insert `imageUrl`, sửa không update `imageUrl`.
- Search theo `name like N'{searchString}'` không có wildcard `%`.
- Permission key có khả năng không đồng nhất: `"18SK "` vs `"18SK"`.

## Vấn đề logic, bảo mật, hiệu suất cần sửa

### Ưu tiên cao

- Chặn hoặc loại bỏ đường vào `MODULE/Sukien` nếu module này không còn là flow chính. Nếu vẫn cần giữ, nên redirect sang `MODULE/Event`.
- Không cho `Sukien` sửa `price` hoặc xóa event khi event đã có đăng ký/phiếu thu, trừ khi tái sử dụng rule từ `MODULE/Event`.
- Thêm lọc dữ liệu theo chi nhánh/quyền trong `Load_list()`.
- Sửa lỗi SQL injection: thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query, ít nhất cho `id`, `searchString`, `code`, `name`, `price`, ngày tháng.
- Escape/serialize JSON đúng cách thay vì tự nối chuỗi response.

### Ưu tiên trung bình

- Đồng nhất permission key `"18SK "` và `"18SK"`.
- Update `imageUrl` trong nhánh sửa hoặc bỏ field khỏi form nếu không còn dùng.
- Sửa search thành contains search nếu đúng kỳ vọng: `name like N'%...%'`.
- Không query `distinct Code` rồi query từng code; dùng một query danh sách event rồi group trên client/server nếu thật sự cần nhóm.
- Dùng `DateTime.Today` hoặc rule ngày rõ ràng thay vì `DateTime.Now` nếu trạng thái chỉ dựa trên ngày.

### Ưu tiên thấp

- Đổi tên title/module trong tài liệu hoặc UI để phân biệt rõ đây là module legacy.
- Chuẩn hóa tên cột `Code`/`code`, `Enable`/`enable` trong code để dễ đọc.
- Thay confirm browser mặc định bằng confirm UI thống nhất nếu vẫn duy trì màn này.

## Khi cần thêm tính năng

Nếu thêm trường mới cho sự kiện, cần sửa tối thiểu:

1. Form trong `MODULE/Sukien/Danhsach.ascx`.
2. JSON fill edit trong `GetEventById()`.
3. Insert/update trong `Savechange()`.
4. Table/list HTML trong `Load_list()` nếu trường đó cần hiển thị.
5. Database schema bảng `Event`.
6. Kiểm tra tương thích với `MODULE/Event`, đặc biệt các cột `ExpireRegistration`, `[Type]`, `quantity`, `AllowCustom`, `replay`.

Nếu thêm filter mới:

1. Thêm control UI trong `.ascx`.
2. Truyền tham số trong `Loadlist()`.
3. Đọc tham số trong `Load_list()`.
4. Thêm điều kiện SQL.
5. Đảm bảo filter theo chi nhánh/quyền nếu là dữ liệu nhạy cảm.

Nếu vẫn duy trì module này:

1. Xác nhận đây là legacy read-only hay vẫn cho CRUD.
2. Nếu còn CRUD, cần đồng bộ rule nghiệp vụ với `MODULE/Event`.
3. Nếu không còn dùng, nên ẩn khỏi route/menu và cân nhắc redirect sang `event!list`.
