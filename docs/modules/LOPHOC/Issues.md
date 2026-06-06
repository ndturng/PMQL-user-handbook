# Issues module Lớp học

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Nhiều file nhận tham số từ request rồi **nối trực tiếp vào SQL**:

- `lophoc.ascx.cs`
- `HocsinhIn_Class.ascx.cs`
- `Lophoc_TKB.ascx.cs`
- `TKB_Statics.ascx.cs`
- `Diemdanh.ascx.cs`
- `Nhapdiem.ascx.cs`
- `Loc_ketqua.ascx.cs`
- `Phonghoc.ascx.cs`
- các form `Addnew_auto.ascx.cs`

Cần sửa lỗi SQL injection bằng cách thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query hoặc helper DB có bind parameter.

### Logic ngày kết thúc học viên không thống nhất

Cùng một khái niệm “học viên còn học/hết lớp” nhưng được tính ở nhiều nơi:

- `lophoc.ascx.cs`
- `HocsinhIn_Class.ascx.cs`
- `Loc_ketqua.ascx.cs`
- `thongke.ascx.cs`

Các công thức khác nhau về:

- số ngày mặc định
- cách cộng ngày nghỉ
- cách xét gia hạn
- cách xét bảo lưu
- khoảng đệm 7 ngày sau kết thúc

Nên gom thành một service/helper dùng chung, sau đó cập nhật các màn hình để cùng đọc một kết quả.

### `Lophoc_join` bị hard delete

`HocsinhIn_Class.ascx.cs` có thao tác xóa khỏi `Lophoc_join` bằng `delete`. Đây là bảng trung tâm của:

- sĩ số lớp
- điểm danh
- kết quả cuối khóa
- thống kê
- lịch sử học viên

Nên đổi sang soft delete hoặc có bảng audit/lịch sử chuyển lớp.

### Cập nhật `Lop_TKB` không có transaction

`Lophoc_TKB.ascx.cs` xóa toàn bộ lịch cũ rồi insert lại lịch mới. Nếu lỗi giữa quá trình lưu, lớp có thể mất lịch.

Cần:

- dùng transaction
- validate dữ liệu trước khi delete
- cân nhắc upsert theo từng dòng lịch

## Mức ưu tiên trung bình

### Chưa kiểm tra trùng lịch giáo viên/phòng học

Khi cấu hình `Lop_TKB`, chưa thấy kiểm tra:

- một giáo viên dạy hai lớp cùng giờ
- một phòng học được xếp cho hai lớp cùng giờ
- lớp trùng lịch trong cùng ngày

Nên bổ sung kiểm tra trước khi lưu lịch.

### Source of truth điểm danh/kết quả chưa rõ

Điểm danh:

- `Lophoc_diemdanh`
- `Diemdanh_style1`
- `Diemdanh_style2`

Kết quả cuối khóa:

- `BangDiem`
- `Ketqua_style1`
- `Ketqua_style2`

Cần xác định bảng nào là nguồn dữ liệu chính cho từng báo cáo, in ấn, email và API/màn hình.

### Permission key có khoảng trắng dư

Một số file dùng permission key có trailing spaces:

- `03KQN  `
- `03CKGK `
- `03TK `

Điều này có thể làm check quyền sai nếu hệ thống quyền không trim key.

### `TKB_Risk` chưa được dùng nhất quán

Một số helper chỉ kiểm tra weekday mà chưa loại ngày nghỉ rõ ràng. Kết quả là:

- danh sách ngày điểm danh có thể lệch ngày nghỉ
- thống kê lịch có thể hiển thị buổi không nên học
- ngày kết thúc học viên có thể bị tính khác nhau giữa màn hình

### Email kết quả cuối khóa gắn với style cụ thể

Flow gửi email trong `Nhapdiem.ascx.cs` đọc dữ liệu kiểu `Ketqua_style1`. Nếu chương trình dùng style khác, cần kiểm tra lại.

## Mức ưu tiên thấp nhưng nên dọn

### Code cũ/lệch trách nhiệm trong `App_Code/BO/lophoc.cs`

Class `LOPHOC` có hàm tên liên quan lớp học nhưng một số phần còn thao tác bảng `chuongtrinh`. Nên tách hoặc xóa nếu không còn dùng.

### JSON/HTML response build thủ công

Nhiều response Ajax đang build HTML/JSON thủ công, có nguy cơ lỗi encode và XSS nếu dữ liệu người dùng nhập không được escape đúng.

### Thư mục in ấn và asset lớn

Các thư mục `Print_TKB`, `Diemdanh/*/Print`, `Ketqua/*/Print` chứa nhiều template/asset. Nếu sửa flow in, nên rà riêng từng template thay vì sửa theo cảm tính.

## Đề xuất hướng sửa

1. Tạo helper/service tính trạng thái học viên trong lớp.
2. Chuẩn hóa source of truth cho điểm danh và kết quả cuối khóa.
3. Sửa lỗi SQL injection ở các command nhận request bằng parameterized query.
4. Thêm transaction cho lưu thời khóa biểu và lưu hàng loạt điểm danh/điểm.
5. Thêm validate trùng lịch giáo viên/phòng học.
6. Chuẩn hóa permission key bằng cách trim và sửa dữ liệu cấu hình nếu cần.
