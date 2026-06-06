# Đăng ký và thanh toán Thi TQ 2024

Nhóm này tương tự `ThiTQ/DangKyThanhToan.md`, gồm đăng ký thí sinh/nhóm, thu khác, thanh toán phí, xác nhận và phiếu thu.

File chính:

- `MODULE/ThiTQ_2024/Dangky_group.ascx.cs`
- `MODULE/ThiTQ_2024/dangky_thitq.ascx.cs`
- `MODULE/ThiTQ_2024/dangky_thukhac.ascx.cs`
- `MODULE/ThiTQ_2024/Danhsach_dangkythiTQ.ascx.cs`
- `MODULE/ThiTQ_2024/Thanhtoan_phi.ascx.cs`
- `MODULE/ThiTQ_2024/xacnhan_thanhtoan.ascx.cs`
- `MODULE/ThiTQ_2024/Phieuthu_hocphi.aspx.cs`

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần transaction cho thanh toán/phiếu thu.
- Cần xác nhận dữ liệu 2024 dùng bảng riêng hay bảng chung với `ThiTQ`.
- Cần chống xác nhận thanh toán lặp.
