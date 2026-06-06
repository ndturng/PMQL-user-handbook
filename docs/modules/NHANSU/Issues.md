# Issues module Nhân sự

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Nhiều file nhận input từ request/form rồi nối trực tiếp vào SQL:

- `Danhsach.ascx.cs`
- `Add_nhansu.ascx.cs`
- `Add_giaovien_auto.ascx.cs`
- `Exchange_nhansu.ascx.cs`
- `TeacherJoinClass.ascx.cs`
- `Update_JoinClass.ascx.cs`
- `Thongke_doanhSo.ascx.cs`
- `THONGKE_Doanhthu.ascx.cs`
- `RATE_GV.ascx.cs`
- `importList*.aspx.cs`
- các file view/form phụ.

Cần sửa lỗi SQL injection bằng cách thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query.

### Insert/update nhiều bảng không có transaction đầy đủ

Các flow có nhiều side effect:

- `Add_nhansu.ascx.cs`: insert `Nhan_su`, insert `Users`, gán quyền.
- `Add_giaovien_auto.ascx.cs`: update `Nhan_su`, sync `Users`.
- `Exchange_nhansu.ascx.cs`: update `Nhan_su`, `Users`, `Lop_TKB`.

Nếu lỗi giữa chừng, hồ sơ nhân sự, tài khoản user, quyền và lịch lớp có thể lệch.

### Quan hệ `Nhan_su` - `Users` chưa được enforce rõ

Danh sách nhân sự đã có cảnh báo:

- nhân sự không có user
- nhân sự gắn nhiều user
- user không active

Điều này cho thấy dữ liệu đã có khả năng lệch. Cần constraint hoặc service trung tâm để quản lý quan hệ này.

### Chuyển cơ sở update lịch lớp hàng loạt

`Exchange_nhansu.ascx.cs` update:

```sql
Update Lop_TKB set idgiaovien = '{tonhansu}' where idgiaovien = '{idnhansu}'
```

Đây là thay đổi lớn vì ảnh hưởng thời khóa biểu lớp. Cần transaction, log/audit và xác nhận rõ phạm vi lớp bị thay đổi.

## Mức ưu tiên trung bình

### Permission key có khoảng trắng dư

Một số check dùng `04NS ` thay vì `04NS`. Nếu hệ thống quyền không trim key, user có thể bị check sai quyền.

### Source of truth chứng nhận chưa rõ

Danh sách nhân sự ưu tiên `GCN_Nhansu`, fallback sang `Nhan_su.NumberCertificate` hoặc `Danhgia_taphuan`.

Cần xác định:

- bảng nào là dữ liệu chứng nhận chính hiện tại
- bảng nào là legacy/fallback
- rule ngày hết hạn tính từ `ngayhethan` hay `ngaycap + 12 tháng`.

### Thống kê doanh thu dùng `Hocphi`

`THONGKE_Doanhthu.ascx.cs` dùng `Dangky_group` join `Hocphi`. Trong docs `KETOAN`, `Hocphi` được xem là dữ liệu cũ ở một số màn. Cần đối chiếu với `ThuChi` để tránh báo cáo lệch.

### Hard delete trong các flow phụ

- `TeacherJoinClass.cancel` delete thẳng.
- Một số import/form phụ có thể insert dữ liệu không qua service chung.

Nên dùng soft delete/audit cho dữ liệu vận hành.

## Mức ưu tiên thấp nhưng nên dọn

### Tên file/class và flow bị trùng/legacy

Có nhiều màn gần giống:

- `Danhsach.ascx.cs`
- `Capnhat.ascx.cs`
- `Add_nhansu.ascx.cs`
- `Add_giaovien_auto.ascx.cs`
- `List_giaovien.ascx.cs`
- `danhsach_giaovien.ascx.cs`

Cần xác định màn nào còn là flow chính, màn nào legacy.

### Response JSON/HTML build thủ công

Nhiều endpoint nối HTML rồi trả JSON/base64/jsonBase. Nên chuẩn hóa serializer và encode output.

### Upload file chưa validate đủ

`Add_giaovien_auto.ascx.cs` có upload file vào `/uploads`. Cần validate extension, MIME, size, quyền upload và path.

## Đề xuất hướng sửa

1. Tạo service trung tâm cho `Nhan_su` - `Users`.
2. Bọc các flow multi-table trong transaction.
3. Sửa SQL injection bằng parameterized query.
4. Chuẩn hóa permission key `04NS` không có khoảng trắng.
5. Làm rõ source of truth chứng nhận/tập huấn.
6. Thêm log/audit khi chuyển cơ sở hoặc thay giáo viên trong `Lop_TKB`.
7. Rà lại báo cáo nhân sự để dùng đúng source doanh thu hiện tại (`ThuChi` hay `Hocphi`).
