# Data model module Danh mục

## Bảng chính

- Bảng tài sản/vật tư.
- Bảng nhóm tài sản/vật tư.
- `KHO` và/hoặc bảng chi tiết phiếu kho.
- `Chinhanh`.
- Bảng ngoại tệ/tỷ giá.

## Issue DB

- Cần xác định bảng nào là source of truth cho vật tư giữa `Danhmuc`, `KHO`, `QuanLyKho`.
- Cần ràng buộc khóa ngoại khi nhập/xuất vật tư.
- Cần lưu lịch sử tỷ giá nếu dùng trong kế toán.
- Cần audit thay đổi danh mục nền.
