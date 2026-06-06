# Khuyến mãi

## Mục đích

Quản lý danh sách và chi tiết khuyến mãi.

File chính:

- `MODULE/Setting/Khuyenmai_add.ascx.cs`
- `MODULE/Setting/List_khuyenmai.ascx.cs`

## DB liên quan

- Bảng khuyến mãi.
- Có thể liên quan học phí, đăng ký học viên, khóa học và kế toán.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần validate thời gian hiệu lực, mức giảm, điều kiện áp dụng.
- Cần xác định khuyến mãi áp dụng theo chi nhánh, khóa học hay toàn hệ thống.
- Cần audit thay đổi vì ảnh hưởng doanh thu.
