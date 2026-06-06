# Học viên trong lớp

## Feature

Feature này hiển thị danh sách học viên đang thuộc một lớp, trạng thái còn học/hết hạn và cho phép gỡ học viên khỏi lớp.

## Trạng thái trong flow chính

Có dùng trong flow chính, nhưng thường đi từ danh sách lớp thay vì menu trực tiếp.

Entry phổ biến:

- Từ `MODULE/LOPHOC/lophoc.ascx.cs` mở màn hình danh sách học viên trong lớp.
- File xử lý chính: `MODULE/LOPHOC/HocsinhIn_Class.ascx.cs`.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/LOPHOC/HocsinhIn_Class.ascx.cs` | Load học viên trong lớp, tính trạng thái kết thúc, gỡ học viên khỏi lớp. |
| `MODULE/LOPHOC/lophoc.ascx.cs` | Link từ danh sách lớp sang danh sách học viên. |
| `MODULE/HOCVIEN/*` | Hồ sơ/đăng ký học viên tạo dữ liệu đầu vào cho `Lophoc_join`. |

## Logic chính

Màn hình này đọc dữ liệu từ `Lophoc_join` để biết học viên nào đang trong lớp.

Các bước chính:

1. Lấy danh sách học viên theo `idlop`.
2. Join sang `Hocsinh`, `Dangky`, `Khoahoc`.
3. Tính ngày kết thúc dự kiến theo `fromdate`, `todate`, ngày nghỉ và gia hạn.
4. Chỉ hiển thị học viên còn đang học theo `check_lopketthuc`.
5. Cho phép `Remove` để xóa học viên khỏi lớp.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Lophoc_join` | Bảng chính của feature, nối học viên/đăng ký với lớp. |
| `Hocsinh` | Hồ sơ học viên. |
| `Dangky` | Đăng ký khóa học. |
| `Khoahoc` | Khóa học đang học. |
| `Giahan_khoahoc` | Gia hạn thời gian học. |
| `Baoluu_khoahoc` | Bảo lưu làm lệch ngày kết thúc thực tế. |
| `Lop_TKB` | Lịch học để tính bù ngày nghỉ. |
| `TKB_Risk` | Ngày nghỉ/ngày rủi ro. |

## Tương tác với module khác

- `HOCVIEN` là nguồn tạo/điều chỉnh đăng ký, bảo lưu, gia hạn; thay đổi ở `HOCVIEN` ảnh hưởng trực tiếp danh sách này.
- `LOPHOC` dùng danh sách này để tính sĩ số lớp.
- `Diemdanh` và `Ketqua` dựa vào `Lophoc_join` để biết học viên nào cần điểm danh/nhập điểm.

## Overlap/xung đột

- Có hàm `Regis_class()` cập nhật `dangky.idlop`, nhưng switch command hiện không thấy gọi rõ ràng; có khả năng là logic cũ còn sót.
- Việc xếp học viên vào lớp có thể tồn tại ở module `HOCVIEN`, còn màn hình này chủ yếu xem/gỡ. Cần tránh sửa một phía làm lệch phía còn lại.
- Logic ngày kết thúc khác với `Loc_ketqua.ascx.cs`, nơi dùng công thức đơn giản hơn.

## Vấn đề cần sửa

- `Remove_class()` đang hard delete khỏi `Lophoc_join`; nên cân nhắc soft delete hoặc log lịch sử vì dữ liệu này ảnh hưởng điểm danh, điểm cuối khóa và thống kê.
- Chưa thấy kiểm tra permission rõ ràng cho thao tác gỡ học viên trong file này.
- **SQL injection do truy vấn SQL nối chuỗi trực tiếp**.
- Cần thống nhất một nguồn tính trạng thái học viên còn học/hết lớp.
