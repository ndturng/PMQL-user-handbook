# Module KHOVATTU (`MODULE/KHOVATTU`)

## Mục đích

`MODULE/KHOVATTU` hiện rất nhỏ, chỉ thấy `home.ascx`. Có khả năng là module cũ/alias cho kho vật tư.

## Trạng thái sử dụng trong app

Không rõ ràng đây là flow chính. Domain kho vật tư hiện rõ hơn ở `MODULE/KHO`, `MODULE/QuanLyKho`, `MODULE/Danhmuc` và `MODULE/KHOAHOC` phần vật tư khóa học.

## Overlap

- Overlap với `KHO` và `QuanLyKho` về quản lý tồn kho/vật tư.
- Overlap với `Danhmuc` nếu liên quan danh mục vật tư/tài sản.

## Vấn đề cần chú ý

Nếu module này còn được route/menu gọi, cần xác định nó là alias hay phần chưa triển khai. Nếu không dùng, nên đánh dấu legacy để tránh dev mới sửa nhầm.
