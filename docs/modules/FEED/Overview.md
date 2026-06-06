# Module Feed (`MODULE/FEED`)

## Mục đích

`MODULE/FEED` quản lý tin/feed nội bộ: tạo feed, danh sách feed, feed đã join, plugin home và màn xem feed.

## Trạng thái sử dụng trong app

Module này có thể đang được dùng trong dashboard/truyền thông nội bộ. `KHOADAOTAO` cũng insert `Feed_news` khi tạo lịch tập huấn, nên feed có tác động liên module.

## Tài liệu con

- [FeedCrud.md](./FeedCrud.md): tạo/sửa/danh sách feed.
- [FeedJointView.md](./FeedJointView.md): join/xem/plugin feed.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro.

## File liên quan

- `MODULE/FEED/home.ascx`
- `MODULE/FEED/menu_top.ascx`
- `MODULE/FEED/Feed_Addnew_auto.ascx.cs`
- `MODULE/FEED/Feed_list.ascx.cs`
- `MODULE/FEED/List_feed.ascx.cs`
- `MODULE/FEED/List_feed1.ascx.cs`
- `MODULE/FEED/Feed_joint.ascx.cs`
- `MODULE/FEED/View_Feed.ascx.cs`
- `MODULE/FEED/Pugin_home.ascx.cs`

## Overlap

- `Notification`: thông báo ngắn/popup.
- `home`: dashboard hiển thị feed/thông báo.
- `KHOADAOTAO`: tạo feed lịch tập huấn.

## Nhận xét nhanh

Feed là kênh truyền thông nội bộ. Cần kiểm tra quyền target chi nhánh, encode nội dung và SQL injection.
