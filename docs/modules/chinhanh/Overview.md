# Module chi nhánh (`MODULE/chinhanh`)

## Mục đích

`MODULE/chinhanh` quản lý chi nhánh/cụm chi nhánh, tài khoản thuộc chi nhánh, học viên của chi nhánh, thanh toán phí sử dụng hệ thống và các màn xác nhận phát sinh từ chi nhánh.

Entry quan sát được:

- `MODULE/chinhanh/home.ascx`
- `MODULE/chinhanh/Danhsach.ascx.cs`
- `MODULE/chinhanh/Add_Chinhanh.ascx.cs`
- `MODULE/chinhanh/Edit_Chinhanh.ascx.cs`
- `MODULE/chinhanh/Add_Cum.ascx.cs`
- `MODULE/chinhanh/Edit_Cum.ascx.cs`

## Trạng thái sử dụng trong app

Đây là module nằm trong flow chính. Dữ liệu `Chinhanh` được dùng rộng ở học viên, lớp học, kế toán, kho, nhân sự, khóa đào tạo, thi TQ và phân quyền user.

## Tài liệu con

- [ChiNhanhCum.md](./ChiNhanhCum.md): CRUD chi nhánh/cụm và cấu hình chi nhánh.
- [HocVienTaiKhoanThanhToan.md](./HocVienTaiKhoanThanhToan.md): học viên, tài khoản, tính tiền, thanh toán và xác nhận.
- [DataModel.md](./DataModel.md): bảng dữ liệu chính và quan hệ.
- [Issues.md](./Issues.md): vấn đề bảo mật, logic, DB và hiệu suất.

## Overlap

- `MODULE/CoSo` cũng hiển thị danh sách/chi tiết cơ sở.
- `MODULE/KETOAN` xử lý thu chi/công nợ/thanh toán, trong khi `chinhanh` có màn thanh toán phí và mua thêm tài khoản.
- `MODULE/USERS` quản lý user, nhưng `chinhanh` cũng có `DS_Users`, `DS_Users_auto`, `DS_Users_info`.
- `MODULE/home` có widget phí/thông báo liên quan chi nhánh.

Khi sửa chi nhánh, cần xác định màn nào là source of truth cho từng dữ liệu: thông tin chi nhánh, hạn sử dụng, số lượng tài khoản, user chi nhánh, công nợ hoặc thanh toán.

## File liên quan

| Nhóm | File |
| --- | --- |
| Chi nhánh/cụm | `Danhsach.ascx.cs`, `Add_Chinhanh.ascx.cs`, `Edit_Chinhanh.ascx.cs`, `Add_Cum.ascx.cs`, `Edit_Cum.ascx.cs`, `Config_Chinhanh.ascx.cs` |
| Học viên chi nhánh | `Ds_hocvien.ascx.cs`, `Confirm_hocvien.ascx.cs`, `View_xacnhan.ascx.cs`, `view/Danhsach_*.ascx.cs` |
| Tài khoản | `Ds_taikhoan.ascx.cs`, `DS_Users.ascx.cs`, `DS_Users_auto.ascx.cs`, `DS_Users_info.ascx.cs` |
| Thanh toán/tính tiền | `Ds_tinhtien.ascx.cs`, `Thanhtoan_phiold.ascx.cs`, `Thanhtoan_muathem.ascx.cs`, `Confirm_payment.ascx.cs` |

## Nhận xét nhanh

Module này không chỉ là danh mục chi nhánh. Nó có side effect sang user, học viên, công nợ, thu chi, notification và hạn sử dụng hệ thống. Nên ưu tiên document/test kỹ trước khi sửa logic thanh toán hoặc số lượng tài khoản.
