# Chi nhánh và cụm chi nhánh

## Mục đích

Nhóm này tạo, sửa, cấu hình và liệt kê chi nhánh/cụm chi nhánh.

File chính:

- `MODULE/chinhanh/Danhsach.ascx.cs`
- `MODULE/chinhanh/Add_Chinhanh.ascx.cs`
- `MODULE/chinhanh/Edit_Chinhanh.ascx.cs`
- `MODULE/chinhanh/Add_Cum.ascx.cs`
- `MODULE/chinhanh/Edit_Cum.ascx.cs`
- `MODULE/chinhanh/Config_Chinhanh.ascx.cs`
- `MODULE/chinhanh/add_new_chinhanh.aspx.cs`

## Logic chính

- Load danh sách chi nhánh/cụm theo quyền user.
- Tạo/sửa thông tin chi nhánh: tên, trạng thái, ngày hết hạn, cấu hình sử dụng.
- Gắn chi nhánh vào cụm để quản lý theo vùng/cụm.
- Cấu hình các thông tin riêng của chi nhánh.

## DB liên quan

- `Chinhanh`: bảng trung tâm.
- Bảng cụm chi nhánh nếu có trong code/data.
- `Users`: user thuộc chi nhánh.
- `Permission_join`: quyền user có thể bị ảnh hưởng khi tạo user chi nhánh.

## Tương tác module khác

- `HOCVIEN`: lọc học viên theo `idChinhanh`.
- `LOPHOC`: lớp/phòng/thời khóa biểu theo chi nhánh.
- `KETOAN`: công nợ, thu chi, học phí, phí hệ thống theo chi nhánh.
- `KHO`/`QuanLyKho`: tồn kho và đơn hàng theo chi nhánh.
- `KHOADAOTAO`, `ThiTQ`: đăng ký/chứng nhận/thống kê theo chi nhánh.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với id chi nhánh/cụm và filter.
- Cần thống nhất soft delete/disable chi nhánh, vì disable chi nhánh có thể kéo theo user/học viên.
- Cần audit thay đổi hạn sử dụng, số lượng tài khoản và trạng thái chi nhánh.
