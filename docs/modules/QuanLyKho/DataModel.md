# Data model `QuanLyKho`

`QuanLyKho` có khả năng dùng chung data model với `KHO`.

## Bảng chính

- `KHO`
- `KHO_Chitietphieu`
- bảng đơn hàng kho
- bảng chi tiết đơn hàng kho
- danh mục vật tư/tài sản
- `Chinhanh`
- `ThuChi` / `Thu_Chi`

## Issue DB

- Cần xác nhận có bảng riêng cho `QuanLyKho` hay dùng chung bảng với `KHO`.
- Nếu dùng chung bảng, hai module phải có cùng rule update trạng thái/tồn kho.
- Nếu dùng bảng riêng, cần document mapping để báo cáo/kế toán không lấy nhầm nguồn.
