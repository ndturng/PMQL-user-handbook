# Thống kê Thi TQ 2024

Nhóm thống kê năm 2024 tương tự `ThiTQ`, gồm thống kê tổng, kết quả, nhìn tỉnh, nghệ tinh, online, review vòng và xếp hạng.

File chính:

- `MODULE/ThiTQ_2024/THONGKE.ascx.cs`
- `MODULE/ThiTQ_2024/THONGKE_Ketqua.ascx.cs`
- `MODULE/ThiTQ_2024/Thongke_thitq.ascx.cs`
- `MODULE/ThiTQ_2024/Thongke_nhintinh.ascx.cs`
- `MODULE/ThiTQ_2024/Thongke_nghetinh.ascx.cs`
- `MODULE/ThiTQ_2024/Thongke_online.ascx.cs`
- `MODULE/ThiTQ_2024/Review_vongnghetinh.ascx.cs`
- `MODULE/ThiTQ_2024/Review_vongonline.ascx.cs`
- `MODULE/ThiTQ_2024/Xephang_thitq.ascx.cs`

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với filter.
- Cần xác định thống kê có đọc dữ liệu 2024 riêng hay bảng chung.
- Cần đối chiếu công thức với `ThiTQ` nếu hai module cùng phục vụ báo cáo.
