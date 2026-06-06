# Module THIETLAP (`MODULE/THIETLAP`)

## Mục đích

`MODULE/THIETLAP` chứa các cấu hình hệ thống nhỏ như banner, phí, template quyền và import danh sách.

## Trạng thái sử dụng trong app

Đây là module cấu hình. Không phải màn nghiệp vụ hằng ngày, nhưng thay đổi dữ liệu tại đây có thể ảnh hưởng rộng.

## File chính

- `MODULE/THIETLAP/Thietlap_banner.ascx.cs`
- `MODULE/THIETLAP/Thietlap_phi.ascx.cs`
- `MODULE/THIETLAP/PermissionTemplate.ascx.cs`
- `MODULE/THIETLAP/importList.aspx.cs`

## DB và dữ liệu liên quan

- Cấu hình phí ảnh hưởng `KETOAN`, `chinhanh`, học phí/thanh toán.
- Permission template ảnh hưởng `USERS`, `NHANSU` và menu.
- Banner ảnh hưởng giao diện chung.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp trong các màn cấu hình.
- Cần audit người sửa và thời điểm sửa vì đây là cấu hình hệ thống.
- Permission template cần được version hóa hoặc có cách rollback.
- Import cần validate file, schema, encoding và quyền upload.

## Ảnh hưởng sang module khác

Sai cấu hình phí/quyền có thể ảnh hưởng đăng nhập, phân quyền, thanh toán và dashboard. Khi sửa module này cần test ít nhất `USERS`, `KETOAN`, `chinhanh` và menu.
