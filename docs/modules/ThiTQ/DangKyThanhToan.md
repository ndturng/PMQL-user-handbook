# Đăng ký và thanh toán thi TQ

## Mục đích

Nhóm này xử lý đăng ký thí sinh/nhóm thi, các khoản thu khác, thanh toán phí thi, xác nhận thanh toán và phiếu thu.

File chính:

- `MODULE/ThiTQ/Dangky_group.ascx.cs`
- `MODULE/ThiTQ/dangky_thitq.ascx.cs`
- `MODULE/ThiTQ/dangky_thukhac.ascx.cs`
- `MODULE/ThiTQ/Danhsach_dangkythiTQ.ascx.cs`
- `MODULE/ThiTQ/Thanhtoan_phi.ascx.cs`
- `MODULE/ThiTQ/xacnhan_thanhtoan.ascx.cs`
- `MODULE/ThiTQ/Phieuthu_hocphi.aspx.cs`
- `MODULE/ThiTQ/Confirm_payment.ascx.cs`

## Logic chính

- Tạo đăng ký thi cho học viên/thí sinh theo chi nhánh.
- Tính phí, chiết khấu, số lượng.
- Upload ảnh chứng từ thanh toán.
- Xác nhận thanh toán và tạo phiếu thu.
- Cập nhật trạng thái đăng ký/thi.

## DB liên quan

- `ThiTQ`
- bảng đăng ký nhóm/chi tiết đăng ký nếu tách riêng.
- `Hocsinh`
- `Chinhanh`
- `ThuChi` / `Thu_Chi`
- bảng thanh toán thi TQ nếu có.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với id đăng ký, số tiền, chiết khấu, số lượng, chi nhánh.
- Cần transaction cho flow thanh toán/xác nhận/phiếu thu.
- Cần validate ảnh chứng từ.
- Cần audit người xác nhận thanh toán.
- Cần chống xác nhận thanh toán lặp.
