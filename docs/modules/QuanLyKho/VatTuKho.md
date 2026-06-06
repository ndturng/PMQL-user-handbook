# Vật tư và tồn kho trong `QuanLyKho`

## Mục đích

Hiển thị và quản lý vật tư/tồn kho theo chi nhánh hoặc HQ.

File chính:

- `MODULE/QuanLyKho/Danhsach.ascx.cs`
- `MODULE/QuanLyKho/Danhsach_HQ.ascx.cs`

## Tương tác

- `Danhmuc`: danh mục vật tư/tài sản.
- `KHO`: module overlap trực tiếp.
- `KETOAN`: giá trị đơn hàng/chi phí.
- `chinhanh`: tồn kho theo cơ sở.

## Vấn đề cần chú ý

- Cần thống nhất source of truth với `KHO`.
- Cần kiểm tra filter chi nhánh/quyền HQ.
- Cần chuẩn hóa đơn vị/hệ số quy đổi.
