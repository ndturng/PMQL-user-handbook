# Tạo và quản lý feed

## Mục đích

Nhóm này tạo feed mới, load danh sách và quản lý nội dung feed.

File chính:

- `MODULE/FEED/Feed_Addnew_auto.ascx.cs`
- `MODULE/FEED/Feed_list.ascx.cs`
- `MODULE/FEED/List_feed.ascx.cs`
- `MODULE/FEED/List_feed1.ascx.cs`

## DB liên quan

- `Feed_news`
- `Feed_news_users`
- `Users`
- `Chinhanh`

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với title/content/date/target.
- Cần encode HTML nội dung feed hoặc dùng editor có whitelist.
- Cần validate target chi nhánh/user.
- Cần audit người tạo/sửa/xóa feed.
