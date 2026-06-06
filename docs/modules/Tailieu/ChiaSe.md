# Chia sẻ tài liệu

## Mục đích

Nhóm này xử lý share link, xem file/folder qua frame hoặc màn xem chi tiết.

File chính:

- `MODULE/Tailieu/Sharelink_auto.ascx.cs`
- `MODULE/Tailieu/frame.ascx`
- `MODULE/Tailieu/View_folder_files.ascx.cs`
- `MODULE/Tailieu/Folder_joint.ascx.cs`

## Logic chính

- Tạo hoặc cập nhật link chia sẻ.
- Hiển thị nội dung file/folder.
- Có thể gán quyền truy cập theo user/chi nhánh/nhóm.

## Vấn đề cần chú ý

- Link chia sẻ cần token khó đoán, hạn dùng và quyền truy cập rõ.
- Không để user đoán id file/folder để xem tài liệu không có quyền.
- Cần log người tạo link và lượt truy cập nếu tài liệu nhạy cảm.
- Cần xử lý file type nguy hiểm khi render qua frame.
