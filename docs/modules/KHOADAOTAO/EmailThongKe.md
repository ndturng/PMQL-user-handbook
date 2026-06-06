# Email và thống kê khóa đào tạo

## Mục đích

Nhóm này gửi kết quả tập huấn và thống kê đăng ký/kết quả/kế toán theo khóa, chi nhánh và thời gian.

File chính:

- `MODULE/KHOADAOTAO/Danhsach.ascx.cs` các command `send`, `sendkq`
- `MODULE/KHOADAOTAO/THONGKE.ascx.cs`
- `MODULE/KHOADAOTAO/Thongke_ketoan.ascx.cs`
- `MODULE/KHOADAOTAO/Thanhtoan_phi.ascx.cs`
- `MODULE/KHOADAOTAO/View_Ketqua.ascx.cs`

## DB liên quan

- `KhoaTapHuan`
- `Giaovien_Taphuan`
- `Danhgia_taphuan`
- `Nhan_su`
- `Users`
- bảng thu chi/phí tập huấn nếu có

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với filter ngày/chi nhánh/khóa.
- Gửi email hàng loạt nên có log trạng thái gửi, retry và tránh timeout request.
- Nội dung email cần encode/format ổn định.
- Báo cáo kế toán cần khớp với flow thanh toán phí tập huấn.
