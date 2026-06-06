# Issues module Danh mục

## Mức ưu tiên cao

- Sửa lỗi SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Xử lý overlap với `KHO`/`QuanLyKho` và `chinhanh`.
- Bọc transaction cho nhập/xuất kho.

## Mức ưu tiên trung bình

- Chuẩn hóa soft delete danh mục.
- Validate đơn vị, đơn giá, số lượng.
- Audit thay đổi danh mục/tỷ giá.
- Tối ưu thống kê nhập/xuất kho.

## Cần test khi sửa

- Tạo/sửa vật tư/tài sản.
- Nhập kho.
- Xuất kho.
- Thống kê nhập/xuất.
- Kiểm tra tồn kho ở `KHO`/`QuanLyKho`.
