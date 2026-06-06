# Câu hỏi, xếp hạng và kết quả Thi TQ 2024

Nhóm này cấu hình câu hỏi/xếp hạng, cập nhật thí sinh và in chứng nhận giống `ThiTQ`.

File chính:

- `MODULE/ThiTQ_2024/Setup_cauhoi.ascx.cs`
- `MODULE/ThiTQ_2024/Add_cauhoi.ascx.cs`
- `MODULE/ThiTQ_2024/Edit_cauhoi.ascx.cs`
- `MODULE/ThiTQ_2024/Setup_xephang.ascx.cs`
- `MODULE/ThiTQ_2024/Add_xephang.ascx.cs`
- `MODULE/ThiTQ_2024/Update_SBD.aspx.cs`
- `MODULE/ThiTQ_2024/Update_ten.aspx.cs`
- `MODULE/ThiTQ_2024/InGCN.ascx.cs`
- `MODULE/ThiTQ_2024/Print/*`

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần khóa scope theo năm/kỳ thi.
- Cần audit thay đổi điểm, SBD, tên và chứng nhận.
- Cần validate import trước khi update dữ liệu thí sinh.
