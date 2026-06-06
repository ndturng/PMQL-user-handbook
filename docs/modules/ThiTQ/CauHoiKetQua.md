# Câu hỏi, xếp hạng và kết quả thi TQ

## Mục đích

Nhóm này cấu hình câu hỏi, xếp hạng, cập nhật số báo danh/tên, nhập điểm/kết quả và in chứng nhận.

File chính:

- `MODULE/ThiTQ/Setup_cauhoi.ascx.cs`
- `MODULE/ThiTQ/Add_cauhoi.ascx.cs`
- `MODULE/ThiTQ/Edit_cauhoi.ascx.cs`
- `MODULE/ThiTQ/Setup_xephang.ascx.cs`
- `MODULE/ThiTQ/Add_xephang.ascx.cs`
- `MODULE/ThiTQ/Update_SBD.aspx.cs`
- `MODULE/ThiTQ/Update_ten.aspx.cs`
- `MODULE/ThiTQ/InGCN.ascx.cs`
- `MODULE/ThiTQ/Print/*`

## Logic chính

- Cấu hình câu hỏi theo năm/vòng thi/bảng thi/khu vực.
- Cấu hình xếp hạng theo khu vực/năm.
- Import/cập nhật số báo danh, vị trí ngồi, bảng thi, tên thí sinh.
- In giấy chứng nhận theo thí sinh/chi nhánh.

## DB liên quan

- `CauhoiThiTQ`
- `ThiTQ_RankNgheOnline`
- `ThiTQ`
- bảng kết quả/chứng nhận nếu tách riêng.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp ở xóa câu hỏi/xếp hạng và import cập nhật.
- Cần validate file import và chống update nhầm thí sinh.
- Cần audit thay đổi kết quả, số báo danh và chứng nhận.
- Cần xác định rule theo năm để không sửa dữ liệu kỳ thi cũ.
