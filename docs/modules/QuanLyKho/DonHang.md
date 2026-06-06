# Đơn hàng trong `QuanLyKho`

## Mục đích

Nhóm đơn hàng trong `QuanLyKho` tương tự `KHO`: tạo/xem đơn hàng, review đơn cũ, đổi trạng thái và cập nhật tồn kho/kế toán.

File chính:

- `MODULE/QuanLyKho/Donhang.ascx.cs`
- `MODULE/QuanLyKho/Donhang_cu.ascx.cs`
- `MODULE/QuanLyKho/Donhang_cureview.ascx.cs`
- `MODULE/QuanLyKho/Donhang/*`

## DB liên quan

Khả năng dùng cùng bảng với `KHO`: đơn hàng kho, chi tiết đơn hàng, `KHO`, `KHO_Chitietphieu`, vật tư, chi nhánh và thu chi.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần transaction cho duyệt đơn/cập nhật tồn/phát sinh kế toán.
- Cần so sánh với `MODULE/KHO` trước khi sửa để tránh khác logic giữa hai module.
