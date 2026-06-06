# Thống kê đăng ký khóa học

## Feature

Feature này thống kê số lượng đăng ký theo chương trình/khóa học trong một khoảng ngày.

Các nhóm số liệu chính:

- Học viên đăng ký/chờ lớp.
- Học viên đã vào lớp.
- Đăng ký bị hủy.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Entry: `Default.aspx?mod=khoahoc!thongke_dangky`
- Permission: `02TK`
- File chính: `MODULE/KHOAHOC/thongke_dangky.ascx.cs`

## Command liên quan

| Command | Vai trò |
| --- | --- |
| `loadCT` | Load chương trình theo chi nhánh. |
| `loadKH` | Load khóa học theo chi nhánh/chương trình. |
| `statics_regis` | Tính thống kê đăng ký. |

## Logic chính

1. User chọn chi nhánh, chương trình, khóa học, khoảng ngày.
2. `loadCT` load chương trình từ `Khoahoc`, `KhoaHoc_ChiNhanh`, `Chuongtrinh`.
3. `loadKH` load khóa học theo chi nhánh và chương trình.
4. `statics_regis`:
   - load danh sách khóa học theo filter.
   - với từng khóa học, tính:
     - `waiting`: đăng ký có `Dangky.enable=1`, `Dangky.status=1` trong khoảng ngày.
     - `regis`: số dòng đã vào lớp dựa trên `Lophoc_join`.
     - `cancel`: đăng ký có `Dangky.enable=0`, `Dangky.status=1`.
   - render table và chart CanvasJS.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Chuongtrinh` | Filter chương trình. |
| `Khoahoc` | Filter khóa học và hiển thị tên/cấp độ. |
| `KhoaHoc_ChiNhanh` | Giới hạn danh sách theo chi nhánh. |
| `Dangky_group` | Chi nhánh của phiếu đăng ký. |
| `Dangky` | Trạng thái đăng ký khóa học. |
| `Lophoc_join` | Xác định học viên đã được đưa vào lớp. |

## Tương tác với module khác

- `HOCVIEN` tạo/cập nhật `Dangky`, `Dangky_group`.
- `LOPHOC`/xếp lớp tạo `Lophoc_join`.
- `KETOAN` có thể dùng số đăng ký để so với học phí/công nợ.

## Overlap/xung đột

- Khái niệm “chờ lớp” ở thống kê đang dựa vào `Dangky`, còn module `LOPHOC` hiện đại dùng nhiều logic qua `Lophoc_join`. Cần kiểm tra khi đối chiếu số liệu.
- File `Wating-khoahoc.ascx.cs`, `List_class.ascx.cs`, `SelectTo_Class.ascx.cs` là flow cũ dùng `Dangky.idlop`, trong khi `LOPHOC` hiện dùng `Lophoc_join` nhiều hơn.
- Thống kê “đã vào lớp” dựa trên `Lophoc_join.updatetime`, không nhất thiết trùng ngày đăng ký hoặc ngày bắt đầu học.

## Vấn đề cần sửa

- Sửa lỗi SQL injection: thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query.
- Làm rõ định nghĩa từng chỉ số: chờ lớp, vào lớp, hủy, theo ngày đăng ký hay ngày xếp lớp.
- Không build chart script bằng nối chuỗi dữ liệu server nếu chưa encode kỹ.
- Nếu dữ liệu lớn, nên aggregate bằng SQL thay vì query từng khóa học rồi count từng loại.
