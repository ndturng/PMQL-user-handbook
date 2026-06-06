# Data model module chi nhánh

## Bảng trung tâm

| Bảng | Vai trò |
| --- | --- |
| `Chinhanh` | Thông tin chi nhánh/cơ sở, hạn dùng, trạng thái, số lượng tài khoản/lớp. |
| `Users` | Tài khoản thuộc chi nhánh. |
| `Hocsinh` | Học viên thuộc chi nhánh. |
| `Lophoc_join` | Quan hệ học viên với lớp/khóa, dùng trong xác nhận học viên. |
| `Thanhtoan_User` | Thanh toán phí hệ thống/mua thêm tài khoản. |
| `ThuChi` / `Thu_Chi` | Phiếu thu/chi phát sinh từ thanh toán. |
| `Congno` | Công nợ phát sinh liên quan chi nhánh/học viên. |
| `Notification` | Thông báo cho chi nhánh/HQ sau các thao tác. |

## Quan hệ quan trọng

- `Users.idChinhanh` -> `Chinhanh.id`
- `Hocsinh.idChinhanh` -> `Chinhanh.id`
- `Lophoc_join.idhocvien` -> `Hocsinh.id`
- `Thanhtoan_User.idChinhanh` -> `Chinhanh.id`
- `Congno.idChiNhanh` -> `Chinhanh.id`

## Issue DB

- Tên bảng `ThuChi` và `Thu_Chi` cần xác nhận đang dùng bảng nào trong từng flow.
- Nhiều update trạng thái chạy rời rạc, thiếu transaction có thể gây lệch dữ liệu giữa thanh toán, hạn dùng và active user.
- Cần xác định rule source of truth cho `Chinhanh.DateExpire`, số lượng user/lớp và trạng thái active.
- Nếu có bảng cụm chi nhánh, cần chuẩn hóa khóa ngoại để tránh chi nhánh mồ côi khi xóa/sửa cụm.
