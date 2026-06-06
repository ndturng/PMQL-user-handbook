# Vật tư và tồn kho

## Mục đích

Nhóm này hiển thị danh sách vật tư/tồn kho cho chi nhánh và HQ.

File chính:

- `MODULE/KHO/Danhsach.ascx.cs`
- `MODULE/KHO/Danhsach_HQ.ascx.cs`

## Logic chính

- Load danh sách vật tư theo chi nhánh hoặc toàn hệ thống.
- Hiển thị tồn kho, đơn vị, đơn giá, định mức.
- Có thể thao tác enable/disable hoặc cập nhật thông tin tồn kho tùy quyền.

## DB liên quan

- `KHO`: tồn kho theo vật tư/chi nhánh.
- `VatTu` hoặc bảng danh mục tương đương.
- `Chinhanh`: phân tách tồn kho theo cơ sở.

## Tương tác module khác

- `Danhmuc` quản lý danh mục vật tư/tài sản.
- `KHOAHOC` gắn vật tư/tài liệu với khóa học.
- `KETOAN` dùng giá trị vật tư/đơn hàng cho báo cáo chi phí.

## Vấn đề cần chú ý

- Cần chuẩn hóa đơn vị và hệ số quy đổi, vì cập nhật tồn kho có thể chia theo hệ số.
- Cần xác định tồn kho âm có được phép không.
- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp ở filter và thao tác update.
