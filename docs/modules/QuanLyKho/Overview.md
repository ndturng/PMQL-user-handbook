# Module Quản lý kho (`MODULE/QuanLyKho`)

## Mục đích

`MODULE/QuanLyKho` có footprint gần như trùng `MODULE/KHO`: đơn hàng vật tư, danh sách tồn kho, thống kê và báo cáo thẻ kho.

## Trạng thái sử dụng trong app

Chưa thể kết luận đây là module chính hay bản legacy/alias nếu chỉ rà nhanh. Cần kiểm tra route/menu production đang trỏ `kho` hay `quanlykho`. Vì code `home.ascx` của hai module có nhiều entry giống nhau, rủi ro overlap rất cao.

## Tài liệu con

- [DonHang.md](./DonHang.md): đơn hàng và cập nhật trạng thái.
- [VatTuKho.md](./VatTuKho.md): vật tư/tồn kho.
- [BaoCao.md](./BaoCao.md): báo cáo kho.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): vấn đề cần xử lý.

## Overlap

Overlap trực tiếp với `MODULE/KHO`. Nếu cả hai còn được dùng, mọi sửa lỗi kho nên được kiểm tra ở cả hai module hoặc hợp nhất ownership.

## File liên quan

- `MODULE/QuanLyKho/home.ascx`
- `MODULE/QuanLyKho/Danhsach.ascx.cs`
- `MODULE/QuanLyKho/Danhsach_HQ.ascx.cs`
- `MODULE/QuanLyKho/Donhang.ascx.cs`
- `MODULE/QuanLyKho/Donhang_cu.ascx.cs`
- `MODULE/QuanLyKho/Donhang_cureview.ascx.cs`
- `MODULE/QuanLyKho/Baocao_chitiet.ascx.cs`
- `MODULE/QuanLyKho/Baocao_thekho.ascx.cs`
- `MODULE/QuanLyKho/Thongke*.ascx.cs`

## Nhận xét nhanh

Ưu tiên đầu tiên không phải thêm tính năng, mà là xác định `QuanLyKho` là module đang dùng, bản copy cũ, hay alias. Nếu không rõ, dev mới rất dễ sửa nhầm module.
