# Module tài liệu (`MODULE/Tailieu`)

## Mục đích

`MODULE/Tailieu` quản lý thư mục, file tài liệu, chia sẻ link và hiển thị cây/tập tin. Module này phục vụ lưu trữ tài liệu nội bộ hoặc tài liệu học tập.

## Trạng thái sử dụng trong app

Có khả năng là module phụ trợ đang dùng trong flow chính nếu menu tài liệu được bật. Nó có nhiều thao tác upload/chia sẻ nên cần xem là module nhạy cảm về file.

## Tài liệu con

- [ThuMucFile.md](./ThuMucFile.md): folder/file CRUD, upload, edit.
- [ChiaSe.md](./ChiaSe.md): share link và xem file/folder.
- [DataModel.md](./DataModel.md): bảng dữ liệu.
- [Issues.md](./Issues.md): rủi ro bảo mật/file.

## File liên quan

- `MODULE/Tailieu/home.ascx`
- `MODULE/Tailieu/menu_top.ascx`
- `MODULE/Tailieu/add_new_folder.ascx.cs`
- `MODULE/Tailieu/folder_edit.ascx.cs`
- `MODULE/Tailieu/add_new_files.ascx.cs`
- `MODULE/Tailieu/edit_file.ascx.cs`
- `MODULE/Tailieu/Folder_joint.ascx.cs`
- `MODULE/Tailieu/show_all.ascx.cs`
- `MODULE/Tailieu/View_folder_files.ascx.cs`
- `MODULE/Tailieu/Sharelink_auto.ascx.cs`
- `MODULE/Tailieu/frame.ascx`

## Overlap

- `KHOAHOC` có tài liệu/vật tư khóa học.
- Các module upload khác dùng `/uploads` nhưng không nhất thiết chung policy.

## Nhận xét nhanh

Ưu tiên lớn nhất là bảo mật file upload/download/share link: validate file, phân quyền xem, chống path traversal và log chia sẻ.
