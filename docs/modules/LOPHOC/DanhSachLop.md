# Danh sách lớp và hồ sơ lớp

## Feature

Feature này quản lý danh sách lớp học, thêm/sửa thông tin lớp và xóa mềm lớp. Đây là điểm vào chính của module `LOPHOC`.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Menu: `Default.aspx?mod=lophoc!home` -> `lophoc!lophoc`
- Permission xem: `03LH`
- Permission thêm: `03LH`, action `2`
- Permission sửa: `03LH`, action `3`
- Permission xóa: `03LH`, action `4`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/LOPHOC/lophoc.ascx.cs` | Load danh sách lớp, filter theo chi nhánh/tên lớp, render sĩ số, xóa mềm lớp. |
| `MODULE/LOPHOC/Lophoc_auto.ascx.cs` | Form động thêm/sửa bảng `lophoc`. |
| `MODULE/LOPHOC/lophoc_addnew.ascx.cs` | Form thêm lớp dạng cũ/khác. |
| `App_Code/BO/lophoc.cs` | Helper `LOPHOC.Load_byid`, `Del_byid`, kiểm tra lịch. |
| `MODULE/LOPHOC/home.ascx` | Dashboard module. |
| `MODULE/LOPHOC/Menu_top.ascx` | Menu con của module. |

## Logic chính

`lophoc.ascx.cs` nhận command Ajax:

- `Load_lophoc`: load danh sách lớp.
- `del_lophoc`: xóa mềm lớp bằng cách cập nhật `Lophoc.enable = 0`.

Danh sách lớp được filter theo:

- `idchinhanh`
- `tenlop`
- `enable = 1`

Mỗi dòng lớp thường hiển thị:

- tên lớp
- chi nhánh
- thời gian bắt đầu/kết thúc
- thời khóa biểu từ `Lop_TKB`
- sĩ số tính từ `Lophoc_join`
- nút sửa thời khóa biểu
- nút xóa lớp

## Cách tính sĩ số

Hàm `siso(idlop)` trong `lophoc.ascx.cs` đếm học viên từ `Lophoc_join` join với `Hocsinh`, chỉ tính học viên còn hiệu lực theo `check_lopketthuc`.

Điểm cần chú ý:

- Có lọc `fromdate` trong khoảng một năm gần đây.
- Có loại học viên đã kết thúc lớp.
- Có tính ngày nghỉ/bảo lưu qua helper `get_totalDayOffs`.
- Logic này không hoàn toàn giống các màn hình khác như `HocsinhIn_Class.ascx.cs` và `Loc_ketqua.ascx.cs`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Lophoc` | Bảng lớp học chính. |
| `Lophoc_join` | Quan hệ học viên/đăng ký với lớp. |
| `Lop_TKB` | Lịch học của lớp. |
| `Hocsinh` | Thông tin học viên để tính sĩ số. |
| `Dangky` | Đăng ký khóa học liên quan đến học viên. |
| `Khoahoc` | Khóa học của đăng ký. |
| `Giahan_khoahoc` | Gia hạn thời gian học. |
| `Baoluu_khoahoc` | Bảo lưu ảnh hưởng ngày kết thúc. |
| `TKB_Risk` | Ngày nghỉ/ngày rủi ro. |

## Overlap/xung đột

- `Lophoc_auto.ascx.cs` và `lophoc_addnew.ascx.cs` cùng liên quan thêm lớp, cần kiểm tra màn hình nào còn dùng trước khi sửa.
- `LOPHOC` trong `App_Code/BO/lophoc.cs` có một số hàm `insert/update` cũ thao tác bảng `chuongtrinh`, tên và trách nhiệm không còn khớp module lớp học.
- Cách tính trạng thái lớp/học viên bị lặp ở nhiều file, dễ lệch kết quả giữa danh sách lớp, danh sách học viên trong lớp và lọc kết quả.

## Vấn đề cần sửa

- SQL injection do truy vấn SQL nối chuỗi trực tiếp từ tham số request.
- Xóa lớp chỉ set `enable=0`, nhưng cần kiểm tra tác động với `Lophoc_join`, `Lop_TKB`, điểm danh và kết quả.
- Logic tính sĩ số nên được gom về một service/helper dùng chung.
- Cần chuẩn hóa tên bảng `Lophoc`/`lophoc` trong code và docs để tránh nhầm trên môi trường DB phân biệt hoa thường.
