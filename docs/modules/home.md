# Module home (`MODULE/home`)

## Mục đích

`MODULE/home` là khu vực dashboard/trang chủ và các widget vận hành: lệnh mới, log, ngoại tệ, phí mới, thống kê, xem khóa học và notification.

## Trạng thái sử dụng trong app

Đây là module thuộc flow chính vì thường là màn đầu sau đăng nhập hoặc dashboard tổng quan.

## File chính

- `MODULE/home/home.ascx.cs`
- `MODULE/home/lenhmoi.ascx.cs`
- `MODULE/home/Log.ascx.cs`
- `MODULE/home/ngoaite.ascx.cs`
- `MODULE/home/Thanhtoan_phinew.ascx.cs`
- `MODULE/home/thongke.ascx.cs`
- `MODULE/home/Viewkhoahoc.ascx.cs`
- `MODULE/home/View_notification.ascx.cs`

## DB và dữ liệu liên quan

Module này đọc dữ liệu tổng hợp từ nhiều domain: thông báo, phí chi nhánh, khóa học, log thao tác, thống kê vận hành và ngoại tệ.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp ở các widget có filter ngày/chi nhánh.
- Dashboard dễ tạo query nặng nếu mỗi widget gọi nhiều truy vấn riêng.
- Cần kiểm soát quyền hiển thị theo chi nhánh/HQ, tránh user thấy dữ liệu toàn hệ thống.
- Cần encode output cho notification/log vì thường chứa nội dung nhập bởi user.

## Ảnh hưởng sang module khác

`home` không phải source of truth dữ liệu, nhưng hiển thị dữ liệu từ nhiều module. Nếu widget sai, user có thể hiểu sai trạng thái phí, thông báo hoặc thống kê dù dữ liệu gốc vẫn đúng.
