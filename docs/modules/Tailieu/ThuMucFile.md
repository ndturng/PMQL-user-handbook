# Thư mục và file tài liệu

## Mục đích

Nhóm này tạo/sửa thư mục, upload/sửa file và hiển thị danh sách file.

File chính:

- `MODULE/Tailieu/add_new_folder.ascx.cs`
- `MODULE/Tailieu/folder_edit.ascx.cs`
- `MODULE/Tailieu/add_new_files.ascx.cs`
- `MODULE/Tailieu/edit_file.ascx.cs`
- `MODULE/Tailieu/show_all.ascx.cs`
- `MODULE/Tailieu/View_folder_files.ascx.cs`

## DB liên quan

- Bảng thư mục tài liệu.
- Bảng file tài liệu.
- `Users`/`Chinhanh` nếu phân quyền theo người tạo hoặc chi nhánh.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với id folder/file, tên, filter.
- Cần validate file upload: extension, MIME, size, tên file, đường dẫn.
- Cần chống path traversal khi lưu/xem file.
- Cần encode tên file/thư mục khi render.
- Cần audit người upload/sửa/xóa.
