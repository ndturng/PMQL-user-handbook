# Module khóa đào tạo (`MODULE/KHOADAOTAO`)

## Mục đích

`MODULE/KHOADAOTAO` quản lý khóa tập huấn/đào tạo nhân sự hoặc giáo viên: tạo khóa, đăng ký tham gia, đánh giá kết quả, chứng nhận, email kết quả và thống kê.

## Trạng thái sử dụng trong app

Module này có dấu hiệu nằm trong flow chính cho đào tạo nội bộ. Nó tương tác trực tiếp với `NHANSU`, `USERS`, `Hocsinh`, `Feed_news` và có thể ảnh hưởng tài khoản đăng nhập sau tập huấn.

## Tài liệu con

- [KhoaTapHuan.md](./KhoaTapHuan.md): tạo/sửa/xóa khóa tập huấn.
- [DangKyDanhGia.md](./DangKyDanhGia.md): đăng ký, đánh giá, chứng nhận, import.
- [EmailThongKe.md](./EmailThongKe.md): email kết quả và thống kê.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro và việc cần sửa.

## File liên quan

| Nhóm | File |
| --- | --- |
| Khóa tập huấn | `Danhsach.ascx.cs`, `Add_Khoataphuan.ascx.cs`, `Add_nhansu.ascx.cs`, `CAPNHAT_Edit.ascx.cs`, `CAPNHAT_Ketthuc.ascx.cs` |
| Đăng ký | `Dangky_taphuan.ascx.cs`, `View_listdangky.ascx.cs`, `View_Ketqua.ascx.cs` |
| Đánh giá/chứng nhận | `CAPNHAT_Danhgia.ascx.cs`, `importListDG.aspx.cs`, `importListGVXS.aspx.cs`, `InCNHQ.ascx.cs` |
| Thống kê/kế toán | `THONGKE.ascx.cs`, `Thongke_ketoan.ascx.cs`, `Thanhtoan_phi.ascx.cs` |

## Overlap

- `NHANSU`: hồ sơ nhân sự/giáo viên.
- `USERS`: tạo/kích hoạt tài khoản sau đánh giá.
- `Event`: cũng dùng dữ liệu giáo viên/tập huấn trong một số flow event.
- `KETOAN`: phí tập huấn/thanh toán.

## Nhận xét nhanh

Module này có nhiều side effect sang tài khoản và hồ sơ nhân sự. Khi sửa đánh giá hoặc xác nhận, cần test cả đăng nhập user, trạng thái nhân sự và chứng nhận.
