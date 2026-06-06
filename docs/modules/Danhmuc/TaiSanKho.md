# Tài sản, vật tư và nhập/xuất kho

## Mục đích

Nhóm này quản lý danh mục tài sản/vật tư, nhập kho, xuất kho và thống kê nhập/xuất.

File chính:

- `MODULE/Danhmuc/taisan.ascx.cs`
- `MODULE/Danhmuc/taisan_add.ascx.cs`
- `MODULE/Danhmuc/taisan_ajax.ascx.cs`
- `MODULE/Danhmuc/taisan_cat_add.ascx.cs`
- `MODULE/Danhmuc/NHAP_KHO.ascx.cs`
- `MODULE/Danhmuc/Xuat_KHO.ascx.cs`
- `MODULE/Danhmuc/Thongke_nhapKho.ascx.cs`
- `MODULE/Danhmuc/Thongke_XuatKho.ascx.cs`

## DB liên quan

- Bảng tài sản/vật tư.
- Bảng nhóm tài sản/vật tư.
- `KHO`, `KHO_Chitietphieu` hoặc bảng nhập/xuất tương đương.
- `Chinhanh`.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần transaction cho nhập/xuất kho và cập nhật tồn.
- Cần chuẩn hóa đơn vị, đơn giá, tồn kho.
- Cần chống nhập/xuất âm hoặc sai chi nhánh.
- Cần audit người nhập/xuất.
