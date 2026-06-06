# Module Cơ sở (`MODULE/CoSo`)

## Mục đích

`MODULE/CoSo` là module xem thông tin cơ sở/chi nhánh ở mức gọn hơn `MODULE/chinhanh`. Module này chủ yếu phục vụ danh sách cơ sở, chi tiết cơ sở và một biến thể danh sách cho kế toán.

File chính:

- `MODULE/CoSo/Danhsach.ascx.cs`
- `MODULE/CoSo/Danhsach_ketoan.ascx.cs`
- `MODULE/CoSo/Detail.ascx.cs`
- `MODULE/CoSo/home.ascx`

## Trạng thái sử dụng trong app

Module này có khả năng vẫn được dùng trong flow quản trị/tra cứu, nhưng không phải source of truth duy nhất cho chi nhánh. `MODULE/chinhanh` mới là module đầy đủ hơn cho CRUD chi nhánh, cụm, user, thanh toán và xác nhận học viên.

## Overlap

- Overlap trực tiếp với `MODULE/chinhanh` về bảng `Chinhanh`.
- Overlap với `KETOAN` khi có màn `Danhsach_ketoan`.
- Nếu sửa thông tin cơ sở, cần xác định sửa ở `CoSo` hay `chinhanh` để tránh hai màn cập nhật lệch nhau.

## DB và dữ liệu liên quan

- Bảng chính: `Chinhanh`.
- Dữ liệu liên quan: `Users`, `Hocsinh`, thanh toán/phí chi nhánh nếu màn kế toán có join.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với tham số query/filter.
- Module nhỏ nhưng dễ gây nhầm vì tên `CoSo` và `chinhanh` cùng quản lý một domain.
- Cần xác định rõ module nào là màn chính trước khi sửa logic chi nhánh.

## Ảnh hưởng sang module khác

Dữ liệu cơ sở/chi nhánh ảnh hưởng đến gần như toàn app: học viên, lớp học, kế toán, kho, nhân sự, khóa đào tạo và phân quyền user.
