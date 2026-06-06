# Lịch nghỉ

## Mục đích

Quản lý ngày nghỉ/lịch nghỉ để ảnh hưởng lịch học, lịch làm việc hoặc hiển thị thông báo.

File chính:

- `MODULE/Setting/Lich_nghi.ascx.cs`
- `MODULE/Setting/Lichnghi_auto.ascx.cs`
- `MODULE/Setting/Lich_nghi_auto.ascx.cs`
- `MODULE/Setting/List_ngaynghi.ascx.cs`

## DB liên quan

- Bảng lịch nghỉ/ngày nghỉ.
- Có thể liên quan `LOPHOC`, `NHANSU`, `home` và thông báo.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần validate ngày bắt đầu/kết thúc và trùng lịch.
- Cần xác định lịch nghỉ toàn hệ thống hay theo chi nhánh.
- Cần kiểm tra tác động đến thời khóa biểu/lịch học.
