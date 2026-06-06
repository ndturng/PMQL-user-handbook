# Data model module khóa đào tạo

## Bảng chính

| Bảng | Vai trò |
| --- | --- |
| `KhoaTapHuan` | Khóa tập huấn/đào tạo. |
| `Giaovien_Taphuan` | Nhân sự/giáo viên đăng ký tham gia khóa. |
| `Danhgia_taphuan` | Kết quả đánh giá, điểm, nhận xét, chứng nhận. |
| `Nhan_su` | Hồ sơ nhân sự/giáo viên. |
| `Users` | Tài khoản đăng nhập liên kết nhân sự. |
| `Feed_news`, `Feed_news_users` | Thông báo lịch tập huấn. |

## Quan hệ quan trọng

- `Giaovien_Taphuan.idKhoa_taphuan` -> `KhoaTapHuan.id`
- `Giaovien_Taphuan.idgiaovien` -> `Nhan_su.id`
- `Danhgia_taphuan.idkhoataphuan` -> `KhoaTapHuan.id`
- `Danhgia_taphuan.idnhansu` -> `Nhan_su.id`
- `Users.idnhansu` -> `Nhan_su.id`

## Issue DB

- Cần ràng buộc chống đăng ký trùng một nhân sự vào cùng khóa.
- Cần chuẩn hóa tên cột điểm vì có dấu hiệu dùng nhiều tên cột điểm khác nhau giữa import và cập nhật.
- Cần lưu audit khi thay đổi kết quả/chứng nhận.
- Cần xác định rule xóa khóa khi đã có đăng ký/đánh giá.
