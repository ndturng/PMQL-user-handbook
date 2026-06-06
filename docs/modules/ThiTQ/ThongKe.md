# Thống kê Thi TQ

## Mục đích

Nhóm thống kê tổng hợp đăng ký, kết quả, xếp hạng và review theo vòng/khu vực/bảng thi/chi nhánh.

File chính:

- `MODULE/ThiTQ/THONGKE.ascx.cs`
- `MODULE/ThiTQ/THONGKE_Ketqua.ascx.cs`
- `MODULE/ThiTQ/Thongke_thitq.ascx.cs`
- `MODULE/ThiTQ/Thongke_nhintinh.ascx.cs`
- `MODULE/ThiTQ/Thongke_nghetinh.ascx.cs`
- `MODULE/ThiTQ/Thongke_online.ascx.cs`
- `MODULE/ThiTQ/Review_vongnghetinh.ascx.cs`
- `MODULE/ThiTQ/Review_vongonline.ascx.cs`
- `MODULE/ThiTQ/Xephang_thitq.ascx.cs`

## DB liên quan

- `ThiTQ`
- `Chinhanh`
- bảng câu hỏi/xếp hạng/kết quả thi TQ.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với filter khu vực, cấp độ, bảng thi, giải, chi nhánh.
- Cần tối ưu query thống kê nếu dữ liệu thi lớn.
- Cần thống nhất công thức xếp hạng giữa review và thống kê.
- Cần kiểm soát quyền xem toàn hệ thống so với chi nhánh.
