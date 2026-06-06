# Module Thi TQ (`MODULE/ThiTQ`)

## Mục đích

`MODULE/ThiTQ` quản lý đăng ký thi TQ, thanh toán phí thi, câu hỏi/xếp hạng, nhập/cập nhật kết quả, in giấy chứng nhận và thống kê theo vòng/khu vực/bảng thi/chi nhánh.

## Trạng thái sử dụng trong app

Module này có dấu hiệu nằm trong flow chính cho kỳ thi TQ. `home.ascx` có nhiều entry dành cho HQ/admin và chi nhánh.

## Tài liệu con

- [DangKyThanhToan.md](./DangKyThanhToan.md): đăng ký, xác nhận, thanh toán, phiếu thu.
- [CauHoiKetQua.md](./CauHoiKetQua.md): câu hỏi, xếp hạng, điểm/kết quả, chứng nhận.
- [ThongKe.md](./ThongKe.md): thống kê thi TQ.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro và việc cần sửa.

## Overlap

- `ThiTQ_2024` có footprint gần như cùng `ThiTQ`. Cần xác định bản nào là source of truth theo kỳ thi/năm.
- `HOCVIEN` và `chinhanh` liên quan học viên/chi nhánh đăng ký.
- `KETOAN` liên quan phiếu thu/thanh toán phí thi.
- `USERS` có màn xem tài khoản thi nếu cấp tài khoản cho thí sinh.

## File liên quan

| Nhóm | File |
| --- | --- |
| Đăng ký | `Danhsach*.ascx.cs`, `Dangky_group.ascx.cs`, `dangky_thitq.ascx.cs`, `dangky_thukhac.ascx.cs` |
| Thanh toán | `Thanhtoan_phi.ascx.cs`, `xacnhan_thanhtoan.ascx.cs`, `Phieuthu_hocphi.aspx.cs`, `Confirm_payment.ascx.cs` |
| Câu hỏi/xếp hạng | `Setup_cauhoi.ascx.cs`, `Add_cauhoi.ascx.cs`, `Edit_cauhoi.ascx.cs`, `Setup_xephang.ascx.cs`, `Add_xephang.ascx.cs` |
| Kết quả/chứng nhận | `THONGKE_Ketqua.ascx.cs`, `Update_SBD.aspx.cs`, `Update_ten.aspx.cs`, `InGCN.ascx.cs`, `Print/*` |
| Thống kê | `THONGKE.ascx.cs`, `Thongke_*.ascx.cs`, `Review_vong*.ascx.cs`, `Xephang_thitq.ascx.cs` |

## Nhận xét nhanh

Đây là module lớn và có nhiều thao tác tài chính/kết quả thi. Khi sửa cần test từ đăng ký đến thanh toán, nhập kết quả, in chứng nhận và thống kê.
