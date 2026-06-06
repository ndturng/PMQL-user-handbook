# Báo cáo kho

## Mục đích

Nhóm báo cáo kho phục vụ thống kê đơn hàng, tồn kho, thẻ kho và báo cáo chi tiết theo chi nhánh/ngày/vật tư.

File chính:

- `MODULE/KHO/Baocao_chitiet.ascx.cs`
- `MODULE/KHO/Baocao_thekho.ascx.cs`
- `MODULE/KHO/Thongke.ascx.cs`
- `MODULE/KHO/Thongke_DH.ascx.cs`
- `MODULE/KHO/Thongke_HQ.ascx.cs`
- `MODULE/KHO/Donhang/Thongke.ascx.cs`
- `MODULE/KHO/Donhang/Thongkechitiet.ascx.cs`

## DB liên quan

- `KHO`
- `KHO_Chitietphieu`
- bảng đơn hàng/chi tiết đơn hàng
- `VatTu`
- `Chinhanh`

## Vấn đề cần chú ý

- Các báo cáo lọc theo ngày/chi nhánh/vật tư cần parameterized query để tránh SQL injection.
- Nếu báo cáo loop từng vật tư/chi nhánh rồi query từng lần, cần tối ưu bằng aggregate SQL.
- Cần thống nhất cách tính tồn đầu, nhập, xuất, tồn cuối giữa báo cáo chi tiết và thẻ kho.

## Ảnh hưởng module khác

Báo cáo kho thường được dùng để đối chiếu với đơn hàng và kế toán. Nếu công thức sai, có thể gây lệch kiểm kê hoặc chi phí vật tư.
