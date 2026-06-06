# Issues module Khóa học

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Nhiều file nối trực tiếp request/form vào SQL:

- `Danhsach.ascx.cs`
- `Chuongtrinh.ascx.cs`
- `Chuongtrinh_Addnew_auto.ascx.cs`
- `Khoahoc_Addnew_auto.ascx.cs`
- `Change_capdo.ascx.cs`
- `khoahoc_chinhanh.ascx.cs`
- `Khoahoc_toCN.aspx.cs`
- `Khoahoc_toCN_edit.ascx.cs`
- `Khoahoc_vattu.aspx.cs`
- `thongke_dangky.ascx.cs`
- `Wating-khoahoc.ascx.cs`
- `List_class*.ascx.cs`
- `SelectTo_Class.ascx.cs`
- `App_Code/BO/khoahoc.cs`
- `App_Code/BO/ChuongTrinh.cs`

Cần sửa lỗi SQL injection bằng cách thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query, đặc biệt với id khóa học/chương trình, chi nhánh, cấp độ, ngày thống kê, học phí.

### Overlap giữa flow lớp cũ và `LOPHOC`

Trong `MODULE/KHOAHOC` còn các file quản lý lớp/chờ lớp:

- `Wating-khoahoc.ascx.cs`
- `List_class.ascx.cs`
- `List_class_end.ascx.cs`
- `SelectTo_Class.ascx.cs`
- `HocsinhIn_Class.ascx.cs`

Các file này dùng nhiều logic dựa trên `Dangky.idlop`, trong khi `LOPHOC` hiện dùng `Lophoc_join` nhiều hơn. Nếu cả hai flow còn truy cập được, dữ liệu lớp/sĩ số/chờ lớp có thể lệch.

### `Chuongtrinh` ảnh hưởng trực tiếp điểm danh/kết quả

`ChuongTrinh.chuongtrinh_bydangky()` và `stylediem_bydangky()` dùng `style_diemdanh`/`style_diem` để quyết định module/bảng động trong `LOPHOC`.

Nếu sửa hoặc xóa chương trình/style sai:

- điểm danh có thể đọc sai bảng `Diemdanh_style*`
- kết quả cuối khóa có thể đọc sai bảng `Ketqua_style*`
- print/email kết quả có thể lỗi.

### Xóa dữ liệu chưa nhất quán

- `KHoahoc.Delkhoahoc()` có thể delete thẳng `Khoahoc` nếu chưa có đăng ký, ngược lại set `enable=0`.
- `Khoahoc_toCN.aspx.cs` hard delete `KhoaHoc_ChiNhanh`.
- `Khoahoc_vattu.aspx.cs` hard delete `Khoahoc_kho`.
- `Chuongtrinh` xóa mềm bằng `enable=0`.

Cần chuẩn hóa soft delete/audit vì các dữ liệu này là cấu hình nền.

## Mức ưu tiên trung bình

### Học phí/số buổi có hai nguồn

- `Khoahoc.HP_full`, `Khoahoc.sogio`: giá/số buổi gốc.
- `KhoaHoc_ChiNhanh.HP_full`, `KhoaHoc_ChiNhanh.sogio`: override theo chi nhánh.

Cần xác định rõ từng flow đọc nguồn nào:

- đăng ký học viên
- thu học phí
- báo cáo
- event target
- hiển thị danh sách.

### Form auto có code leftover sai domain

`Khoahoc_Addnew_auto.ascx.cs`, `Chuongtrinh_Addnew_auto.ascx.cs`, `Khoahoc_toCN_edit.ascx.cs` có đoạn cập nhật `hocsinh.mahs`/biến `mahv` trong flow insert. Đây có vẻ là code copy từ module học viên.

Cần dọn để tránh side effect không liên quan.

### Upload file chưa validate đủ

Các form auto upload file vào `/uploads` nhưng chưa thấy validate rõ:

- extension whitelist
- MIME type
- size
- quyền upload
- path/tên file.

### Hard-code/course id và CSV

- `KETOAN` đang hard-code nhiều course id trong báo cáo tháng.
- `Khoahoc.nextCourse` có vẻ dùng CSV.
- `EventTarget.SubCourse` dùng danh sách khóa học dạng text/CSV.

Cần chuẩn hóa quan hệ nếu muốn query/report ổn định.

### Permission key không thống nhất tên

Home dùng `21QLKH`, code danh mục dùng `02QKDM`, thống kê dùng `02TK`, chi nhánh dùng `Check_QuanLy()`, vật tư dùng `01DKKH`. Cần document quyền thực tế trong hệ thống permission để tránh user có menu nhưng vào màn bị chặn.

## Mức ưu tiên thấp nhưng nên dọn

### Partial class đặt tên sai domain

Một số file trong `MODULE/KHOAHOC` dùng class name như `MODULE_HOCVIEN_Danhsach`. Không gây lỗi nếu compile được, nhưng gây nhầm cho người đọc.

### Response HTML/JSON build thủ công

Nhiều endpoint nối HTML/JSON thủ công và dùng `Server.HtmlEncode`/base64 không nhất quán. Cần chuẩn hóa serializer và encode output.

### Query từng khóa học rồi count từng loại

`thongke_dangky.ascx.cs` loop từng khóa học và gọi nhiều query count. Nếu số khóa học/dữ liệu lớn, nên aggregate một lần bằng SQL.

## Đề xuất hướng sửa

1. Xác định source of truth hiện tại cho xếp lớp: `Lophoc_join` hay `Dangky.idlop`.
2. Sửa lỗi SQL injection trong danh mục và các API cấu hình bằng parameterized query.
3. Chuẩn hóa soft delete cho `Khoahoc`, `KhoaHoc_ChiNhanh`, `Khoahoc_kho`.
4. Tách service cấu hình khóa học/chương trình khỏi code-behind.
5. Thêm constraint/index cho `KhoaHoc_ChiNhanh` và `Khoahoc_kho`.
6. Làm rõ rule giá gốc và giá theo chi nhánh.
7. Validate upload file ở form auto.
8. Dọn code leftover sai domain trong các form auto.
