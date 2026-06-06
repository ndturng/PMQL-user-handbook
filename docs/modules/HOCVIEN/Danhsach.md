# HOCVIEN: Danh sách học viên

## Mục đích

Feature danh sách học viên là màn làm việc chính cho việc tìm kiếm, xem hồ sơ và thao tác nhanh trên học viên.

Entry chính:

```text
Default.aspx?mod=hocvien!Danhsach
```

File chính:

- `MODULE/HOCVIEN/Danhsach.ascx`
- `MODULE/HOCVIEN/Danhsach.ascx.cs`
- `MODULE/HOCVIEN/js/jshocvien.js`
- `MODULE/HOCVIEN/js/jshocsinh.js`

## Trạng thái sử dụng trong app

Đây là flow chính. `home.ascx` và `Menu_top.ascx` đều trỏ tới `hocvien!Danhsach`. Search nhanh từ dashboard cũng redirect về màn này với `txtsearch`.

## Commands

`Danhsach.ascx.cs` xử lý các `cmd` chính:

| Command | Function | Vai trò |
| --- | --- | --- |
| `load` | `Load_list()` | Load danh sách học viên |
| `deleted` | `delete()` | Xóa mềm học viên |
| `changedkcu` | `Change_dkcu()` | Cho phép đăng ký khóa cũ |
| `updateEnd` | `Update_statusEnd()` | Khôi phục kết thúc sớm |
| `Submit_reactive` | `Submit_reactive()` | Kích hoạt lại khóa học bảo lưu |
| `Submit_endsoon` | `Submit_endsoon()` | Kết thúc sớm khóa học |
| `Submit_extend` | `Submit_extend()` | Gia hạn khóa học |
| `endExtend` | `End_extend()` | Kết thúc gia hạn |
| `cancel_khoahoc` | `Cancel_khoahoc()` | Hủy khóa học, tạo voucher |

## Trạng thái học viên trên danh sách

`Load_list()` tự tính trạng thái bằng SQL `case` dựa trên các bảng `Baoluu_khoahoc`, `Dangky`, `Lophoc_join`.

| Trạng thái | Điều kiện chính |
| --- | --- |
| `baoluu` | Có `Baoluu_khoahoc.status=1`, nằm trong khoảng `fromdate` - `todate` |
| `danghoc` | Có `Lophoc_join` còn hạn, chưa `status_end` |
| `choxetlop` | Có `Dangky.enable=1` nhưng chưa có `Lophoc_join` |
| `daketthuc` | Có `Dangky.enable=1` nhưng không còn lớp đang học |
| `dahuy` | Có `Dangky` nhưng không còn active |
| `tiemnang` | Chưa có đăng ký |

Đây là logic quan trọng: trạng thái không nằm ở một cột duy nhất, mà được suy ra từ nhiều bảng.

## Luồng load danh sách

1. User vào `hocvien!Danhsach`.
2. `Page_Load` kiểm tra quyền `Users.Check_PerBymod("01HV", 1)`.
3. Load danh sách chi nhánh user được phép xem.
4. Client gọi `cmd=load`.
5. Server đọc filter: `status`, `ten`, `ctmkt`, `chinhanh`, `sort`, `filter_trangthai`, `page`.
6. Query `Hocsinh` theo chi nhánh, marketing, tên, trạng thái.
7. Render HTML rows và pagination thủ công.
8. Trả JSON `{status:"ok", str:"<encoded html>"}`.

## Action trên từng học viên

| Action | Tác động |
| --- | --- |
| Xem chi tiết | Gọi `View_Detail(id)` |
| Đăng ký khóa học | Link tới `hocvien!dangky_group&idhocvien={id}` |
| Sửa học viên | Mở form edit qua colorbox |
| Xóa học viên | Chỉ hiện với `idChinhanh==1`, update `Hocsinh.enable=0` |
| Đăng ký tham gia event | Gọi flow `MODULE/Event/Manage.ascx.cs` |
| Chuyển cơ sở | Mở flow `Change_Users` |
| Cho phép ĐK khóa cũ | Update `Hocsinh.status_dkcu=0` |

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Hocsinh` | Nguồn danh sách học viên |
| `Dangky` | Xác định tiềm năng/đã đăng ký/đã hủy |
| `Dangky_group` | Phiếu đăng ký nhóm |
| `Lophoc_join` | Xác định đang học/kết thúc/gia hạn |
| `Baoluu_khoahoc` | Xác định đang bảo lưu |
| `Hocsinh_voucher` | Tạo voucher khi hủy khóa |
| `Nhansu_Log` | Log xóa học viên |
| `Users`, `ChiNhanh` | Quản lý user/chi nhánh |
| `Config`, `Config_data_text` | Cấu hình phí cơ sở/marketing |

## Vấn đề cần chú ý

- SQL injection do truy vấn SQL nối chuỗi trực tiếp từ query string: `ten`, `ctmkt`, `chinhanh`, `filter_trangthai`, `iddk`, `id`.
- `Load_list()` lấy toàn bộ dataset rồi phân trang trong memory, không phân trang ở SQL.
- Trạng thái học viên là derived state; nếu chỉ update một bảng rất dễ lệch trạng thái.
- `delete()` update `Hocsinh.enable=0` nhưng không xử lý đăng ký/lớp/học phí liên quan.
- `Cancel_khoahoc()` tạo voucher bằng toàn bộ `Tonghocphi`, cần kiểm tra tình trạng đã thu/đã học trước khi dùng.
- Các thao tác gia hạn/bảo lưu/kết thúc sớm update nhiều bảng nhưng không thấy transaction.
- Flow đăng ký event từ list dùng `event!manage`, có overlap với flow event chính.
