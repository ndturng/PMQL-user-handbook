# Module Setting (`MODULE/Setting`)

## Mục đích

`MODULE/Setting` quản lý cấu hình vận hành như khuyến mãi, lịch nghỉ, data text/config và menu thiết lập.

## Trạng thái sử dụng trong app

Đây là module cấu hình. Không phải flow nhập liệu hằng ngày, nhưng dữ liệu tại đây có thể ảnh hưởng đăng ký, học phí, lịch học và hiển thị nội dung.

## Tài liệu con

- [KhuyenMai.md](./KhuyenMai.md): cấu hình khuyến mãi.
- [LichNghi.md](./LichNghi.md): lịch nghỉ/ngày nghỉ.
- [ConfigDataText.md](./ConfigDataText.md): cấu hình text/data.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): vấn đề cần sửa.

## File liên quan

- `MODULE/Setting/home.ascx`
- `MODULE/Setting/Menu_top.ascx`
- `MODULE/Setting/Khuyenmai_add.ascx.cs`
- `MODULE/Setting/List_khuyenmai.ascx.cs`
- `MODULE/Setting/Lich_nghi.ascx.cs`
- `MODULE/Setting/Lichnghi_auto.ascx.cs`
- `MODULE/Setting/Lich_nghi_auto.ascx.cs`
- `MODULE/Setting/List_ngaynghi.ascx.cs`
- `MODULE/Setting/Config_data_text.ascx.cs`

## Overlap

- `THIETLAP` cũng chứa cấu hình hệ thống.
- `KETOAN`/`HOCVIEN` có thể dùng khuyến mãi.
- `LOPHOC` có thể bị ảnh hưởng bởi lịch nghỉ.
