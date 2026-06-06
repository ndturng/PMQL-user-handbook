# Đơn hàng kho

## Mục đích

Nhóm đơn hàng xử lý đặt vật tư, xem đơn cũ, review/duyệt và cập nhật tồn kho sau khi trạng thái đơn thay đổi.

File chính:

- `MODULE/KHO/Donhang.ascx.cs`
- `MODULE/KHO/Donhang_cu.ascx.cs`
- `MODULE/KHO/Donhang_cureview.ascx.cs`
- `MODULE/KHO/Donhang/Status_donhang.ascx.cs`
- `MODULE/KHO/Donhang/Thongke.ascx.cs`
- `MODULE/KHO/Donhang/Thongkechitiet.ascx.cs`

## Logic chính

- Chi nhánh tạo/xem đơn hàng vật tư.
- HQ hoặc user có quyền duyệt trạng thái đơn.
- Khi duyệt, hệ thống có thể ghi chi tiết phiếu kho, update tồn kho và phát sinh thu chi.
- Thống kê đơn hàng theo ngày/chi nhánh.

## DB liên quan

- Bảng đơn hàng và chi tiết đơn hàng.
- `KHO`, `KHO_Chitietphieu`: tồn kho và chi tiết phiếu nhập/xuất.
- `VatTu`: danh mục vật tư.
- `Chinhanh`: đơn hàng theo chi nhánh.
- `ThuChi`/`Thu_Chi`: phát sinh kế toán khi thanh toán đơn.

## Vấn đề cần chú ý

- Cần transaction khi đổi trạng thái đơn và cập nhật tồn kho/kế toán.
- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với id đơn, ngày, chi nhánh.
- Cần chống duyệt lặp dẫn đến cộng/trừ tồn kho nhiều lần.
- Cần audit người duyệt, thời điểm duyệt, trạng thái trước/sau.
