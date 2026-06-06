# Báo cáo trong `QuanLyKho`

## Mục đích

Báo cáo chi tiết kho, thẻ kho và thống kê đơn hàng/tồn kho.

File chính:

- `MODULE/QuanLyKho/Baocao_chitiet.ascx.cs`
- `MODULE/QuanLyKho/Baocao_thekho.ascx.cs`
- `MODULE/QuanLyKho/Thongke.ascx.cs`
- `MODULE/QuanLyKho/Thongke_DH.ascx.cs`
- `MODULE/QuanLyKho/Thongke_HQ.ascx.cs`

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với filter ngày/chi nhánh/vật tư.
- Cần so sánh công thức báo cáo với `KHO`.
- Cần tối ưu query nếu báo cáo chạy nhiều vòng lặp theo vật tư/chi nhánh.
