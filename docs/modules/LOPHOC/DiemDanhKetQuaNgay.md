# Điểm danh và kết quả ngày

## Feature

Feature này theo dõi kết quả học/ngày học của học viên trong lớp. Trong code có hai hướng lưu dữ liệu:

- Bảng cố định `Lophoc_diemdanh`.
- Bảng động theo chương trình như `Diemdanh_style1`, `Diemdanh_style2`.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Entry: `lophoc!diemdanh`
- Permission chính: `03KQN`
- Một số form động dùng permission key có khoảng trắng dư như `03KQN  `.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/LOPHOC/Diemdanh.ascx.cs` | Màn hình danh sách điểm danh/kết quả ngày. |
| `MODULE/LOPHOC/Diemdanh/Diemdanh_style1/INSERT/Addnew_auto.ascx.cs` | Form động nhập/sửa điểm danh style 1. |
| `MODULE/LOPHOC/Diemdanh/Diemdanh_style2/INSERT/Addnew_auto.ascx.cs` | Form động nhập/sửa điểm danh style 2. |
| `MODULE/LOPHOC/Diemdanh/*/Print/*` | In phiếu/bảng điểm danh. |
| `App_Code/BO/lophoc.cs` | Kiểm tra ngày học hợp lệ. |

## Logic chính

`Diemdanh.ascx.cs` xử lý các command:

- `CP_time`: trả danh sách ngày có thể chọn theo lịch học.
- `Load_diemdanh`: load grid điểm danh/kết quả theo lớp và khoảng ngày.
- `SaveList`: lưu nhanh vào `Lophoc_diemdanh`.

Khi load dữ liệu, code dùng `ChuongTrinh.chuongtrinh_bydangky(iddangky)` để lấy `modulediemdanh`, sau đó đọc bảng động tương ứng. Vì vậy bảng dữ liệu thật phụ thuộc vào cấu hình chương trình của đăng ký.

Form `Addnew_auto` của từng style dùng `Form_auto` để sinh input và insert/update bảng động. Điều kiện nhận diện bản ghi thường dựa trên:

- `iddangky`
- `idhocsinh`
- `ngay`

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Lophoc_diemdanh` | Bảng điểm danh cố định, được `SaveList` upsert. |
| `Diemdanh_style1`, `Diemdanh_style2` | Bảng động nhập chi tiết theo cấu hình chương trình. |
| `Lophoc_join` | Nguồn học viên trong lớp. |
| `Lophoc` | Thông tin lớp. |
| `Lop_TKB` | Xác định ngày học. |
| `ChuongTrinh` | Chọn module/bảng điểm danh. |
| `TKB_Risk` | Ngày nghỉ/ngày rủi ro. |

## Tương tác với module khác

- `ChuongTrinh` quyết định style điểm danh.
- `HOCVIEN` và `Lophoc_join` quyết định học viên nào xuất hiện trong danh sách.
- `ThoiKhoaBieu` quyết định ngày nào có thể điểm danh.
- Dữ liệu điểm danh có thể được dùng để in báo cáo/phiếu kết quả.

## Overlap/xung đột

- `Lophoc_diemdanh` và `Diemdanh_style*` cùng chứa dữ liệu liên quan điểm danh/kết quả ngày. Chưa rõ bảng nào là source of truth cuối cùng.
- `SaveList` lưu vào bảng cố định, nhưng màn hình load có lúc đọc bảng động theo `modulediemdanh`.
- Style điểm danh phụ thuộc vào cấu hình chương trình, dễ lỗi nếu `modulediemdanh` trỏ sai bảng hoặc bảng thiếu field.

## Vấn đề cần sửa

- Cần xác định source of truth cho điểm danh.
- **SQL injection do truy vấn SQL nối chuỗi trực tiếp**.
- Permission key có khoảng trắng dư ở một số form.
- Cần transaction/upsert rõ ràng khi lưu hàng loạt.
- Cần thống nhất xử lý ngày nghỉ `TKB_Risk` khi sinh danh sách ngày điểm danh.
