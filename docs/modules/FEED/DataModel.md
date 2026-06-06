# Data model module Feed

## Bảng chính

| Bảng | Vai trò |
| --- | --- |
| `Feed_news` | Nội dung feed/tin. |
| `Feed_news_users` | Target chi nhánh/user nhận feed. |
| `Users` | Người tạo/xem feed. |
| `Chinhanh` | Target chi nhánh. |

## Issue DB

- Cần xác định feed target theo chi nhánh, user hay cả hai.
- Cần trạng thái đã đọc/ẩn/xóa nếu nghiệp vụ cần.
- Cần audit người tạo/sửa/xóa.
- Cần index theo target và ngày để plugin home không chậm.
