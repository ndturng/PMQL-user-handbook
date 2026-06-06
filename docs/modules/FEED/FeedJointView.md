# Join và xem feed

## Mục đích

Nhóm này xử lý user/chi nhánh nhận feed, xem feed và plugin hiển thị ở home.

File chính:

- `MODULE/FEED/Feed_joint.ascx.cs`
- `MODULE/FEED/View_Feed.ascx.cs`
- `MODULE/FEED/Pugin_home.ascx.cs`

## DB liên quan

- `Feed_news`
- `Feed_news_users`
- `Users`
- `Chinhanh`

## Vấn đề cần chú ý

- Cần kiểm tra quyền xem feed theo target.
- Cần chống user đoán id feed để xem nội dung không thuộc mình.
- Cần tối ưu plugin home nếu load feed mỗi lần vào dashboard.
- Cần encode output nội dung feed.
