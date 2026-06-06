# Module kho (`MODULE/KHO`)

## Mục đích

`MODULE/KHO` quản lý kho vật tư: danh sách tồn kho, đơn hàng vật tư, thống kê và báo cáo thẻ kho. Đây là module vận hành có liên quan trực tiếp đến chi nhánh, kế toán và danh mục vật tư.

## Trạng thái sử dụng trong app

Module này có dấu hiệu nằm trong flow chính. `home.ascx` có các entry theo permission như quản lý đơn hàng, quản lý vật tư, thẻ kho và thống kê.

## Tài liệu con

- [DonHang.md](./DonHang.md): đơn hàng, duyệt trạng thái, cập nhật tồn kho.
- [VatTuKho.md](./VatTuKho.md): danh sách vật tư/tồn kho.
- [BaoCao.md](./BaoCao.md): thống kê, báo cáo chi tiết, thẻ kho.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro và việc cần sửa.

## Overlap

`MODULE/QuanLyKho` có footprint gần như trùng `MODULE/KHO`. Cần xác định module nào đang được route/menu chính dùng. Nếu cả hai cùng active, nguy cơ sửa một module nhưng user lại dùng module còn lại.

## File liên quan

| Nhóm | File |
| --- | --- |
| Entry/menu | `home.ascx`, `Menu_top.ascx` |
| Đơn hàng | `Donhang.ascx.cs`, `Donhang_cu.ascx.cs`, `Donhang_cureview.ascx.cs`, `Donhang/*` |
| Vật tư/tồn kho | `Danhsach.ascx.cs`, `Danhsach_HQ.ascx.cs` |
| Báo cáo | `Baocao_chitiet.ascx.cs`, `Baocao_thekho.ascx.cs`, `Thongke.ascx.cs`, `Thongke_DH.ascx.cs`, `Thongke_HQ.ascx.cs` |

## Tương tác module khác

- `Danhmuc`: danh mục vật tư/tài sản, nhập/xuất kho.
- `KETOAN`: tạo phiếu thu/chi khi thanh toán đơn hàng.
- `chinhanh`: tồn kho/đơn hàng theo chi nhánh.
- `KHOAHOC`: vật tư/tài liệu gắn với khóa học.
