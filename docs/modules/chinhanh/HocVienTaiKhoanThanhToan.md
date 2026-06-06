# Học viên, tài khoản và thanh toán chi nhánh

## Mục đích

Nhóm này xử lý dữ liệu vận hành của một chi nhánh: học viên, user, tài khoản mua thêm, tính tiền, xác nhận phí và phát sinh công nợ.

File chính:

- `MODULE/chinhanh/Ds_hocvien.ascx.cs`
- `MODULE/chinhanh/Ds_taikhoan.ascx.cs`
- `MODULE/chinhanh/DS_Users.ascx.cs`
- `MODULE/chinhanh/Confirm_hocvien.ascx.cs`
- `MODULE/chinhanh/View_xacnhan.ascx.cs`
- `MODULE/chinhanh/Ds_tinhtien.ascx.cs`
- `MODULE/chinhanh/Thanhtoan_phiold.ascx.cs`
- `MODULE/chinhanh/Thanhtoan_muathem.ascx.cs`
- `MODULE/chinhanh/Confirm_payment.ascx.cs`

## Logic chính

- Xem học viên/tài khoản theo chi nhánh.
- Xác nhận học viên hoặc cập nhật trạng thái học viên.
- Tính tiền phí hệ thống/phát sinh.
- Ghi nhận thanh toán phí chi nhánh hoặc mua thêm tài khoản.
- Cập nhật hạn dùng chi nhánh, kích hoạt user/học viên và tạo thông báo.

## DB liên quan

- `Chinhanh`: hạn dùng, số lượng tài khoản/lớp, trạng thái.
- `Users`: tài khoản nhân sự/user chi nhánh.
- `Hocsinh`, `Lophoc_join`: học viên và trạng thái học/lớp.
- `Thanhtoan_User`: thanh toán phí/mua thêm.
- `ThuChi` hoặc `Thu_Chi`: phiếu thu/chi phát sinh.
- `Congno`: công nợ phát sinh.
- `Notification`: thông báo sau thanh toán/xác nhận.

## Tương tác module khác

- `KETOAN` cần dữ liệu thu chi/công nợ đúng để báo cáo.
- `USERS` chịu ảnh hưởng khi active/exp user thay đổi.
- `HOCVIEN` chịu ảnh hưởng khi học viên được enable/xác nhận.
- `home` và `Notification` hiển thị trạng thái phí/thông báo.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp từ query string như `idedit`, `id`, `sotien`, `sothang`, `imageup`.
- Cần transaction cho các flow vừa insert thanh toán vừa update chi nhánh/user/học viên/notification.
- Cần validate số tiền, số tháng, số lượng tài khoản, file ảnh chứng từ.
- Cần kiểm tra quyền duyệt thanh toán riêng với quyền chỉ xem danh sách.
- Các thao tác update hạn dùng và active user/học viên có blast radius lớn, cần audit.
