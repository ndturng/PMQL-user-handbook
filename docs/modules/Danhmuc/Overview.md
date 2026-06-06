# Module Danh mục (`MODULE/Danhmuc`)

## Mục đích

`MODULE/Danhmuc` chứa các danh mục và thao tác nền liên quan chi nhánh, ngoại tệ, tài sản/vật tư, nhập kho, xuất kho và thống kê kho.

## Trạng thái sử dụng trong app

Đây là module cấu hình/danh mục phụ trợ nhưng ảnh hưởng trực tiếp kho, kế toán và chi nhánh. Không nên xem là module nhỏ dù số file vừa phải.

## Tài liệu con

- [TaiSanKho.md](./TaiSanKho.md): tài sản/vật tư, nhập/xuất kho, thống kê.
- [NgoaiTeChiNhanh.md](./NgoaiTeChiNhanh.md): chi nhánh, ngoại tệ.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro.

## File liên quan

- `MODULE/Danhmuc/chinhanh.ascx.cs`
- `MODULE/Danhmuc/ngoaite.ascx.cs`
- `MODULE/Danhmuc/taisan.ascx.cs`
- `MODULE/Danhmuc/taisan_add.ascx.cs`
- `MODULE/Danhmuc/taisan_ajax.ascx.cs`
- `MODULE/Danhmuc/taisan_cat_add.ascx.cs`
- `MODULE/Danhmuc/NHAP_KHO.ascx.cs`
- `MODULE/Danhmuc/Xuat_KHO.ascx.cs`
- `MODULE/Danhmuc/Thongke_nhapKho.ascx.cs`
- `MODULE/Danhmuc/Thongke_XuatKho.ascx.cs`

## Overlap

- `KHO`/`QuanLyKho`: tồn kho, nhập/xuất, vật tư.
- `chinhanh`/`CoSo`: danh mục chi nhánh.
- `home`: ngoại tệ.
- `KETOAN`: nhập/xuất kho có thể phát sinh giá trị kế toán.
