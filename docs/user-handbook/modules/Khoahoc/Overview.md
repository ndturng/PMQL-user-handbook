# Sổ tay người dùng: Khóa học

## Mục đích

Module Khóa học dùng để quản lý chương trình học, khóa học, cấu hình khóa theo cơ sở, vật tư/tài liệu đi kèm và thống kê số lượng đăng ký khóa học.

Module này là dữ liệu nền cho nhiều luồng khác trong hệ thống:

- đăng ký học viên
- xếp lớp
- điểm danh/kết quả học tập
- thu học phí
- lọc học viên theo khóa học khi tạo sự kiện
- báo cáo đăng ký và vận hành lớp

Đường dẫn sử dụng chính:

```text
Khóa học
```

## Các khu vực chính

| Khu vực | Mục đích |
| --- | --- |
| Chương Trình, Khóa Học | Quản lý danh mục chương trình và khóa học. |
| Chương Trình Theo Cơ Sở | Cấu hình khóa học nào được áp dụng tại từng cơ sở, học phí, số giờ và các phí liên quan. |
| Thống kê đăng ký | Xem số lượng đăng ký, xếp lớp và hủy khóa theo chi nhánh, chương trình, khóa học và khoảng ngày. |

## Điều kiện chung để sử dụng

Người dùng cần có quyền truy cập module Khóa học. Một số thao tác chỉ hiển thị với tài khoản quản lý hoặc tài khoản có phân quyền riêng:

- thêm/sửa/xóa chương trình
- thêm/sửa/xóa khóa học
- cài đặt trạng thái khóa học
- gắn vật tư/tài liệu cho khóa học
- phân bổ khóa học về cơ sở
- xem thống kê đăng ký

## Tài liệu con

- [QuanLyChuongTrinhKhoaHoc.md](./QuanLyChuongTrinhKhoaHoc.md): quản lý chương trình, khóa học, cấp độ và cài đặt khóa học.
- [PhanBoTheoCoSo.md](./PhanBoTheoCoSo.md): thêm khóa học vào cơ sở và chỉnh học phí/số giờ/phí online/phí test.
- [VatTuTaiLieu.md](./VatTuTaiLieu.md): gắn vật tư hoặc tài liệu đi kèm khóa học.
- [ThongKeDangKy.md](./ThongKeDangKy.md): thống kê đăng ký, xếp lớp và hủy khóa.
- [LuuYVanHanh.md](./LuuYVanHanh.md): lưu ý vận hành và checklist trước khi sửa dữ liệu nền.
