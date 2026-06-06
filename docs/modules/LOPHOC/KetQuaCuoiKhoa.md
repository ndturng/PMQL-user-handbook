# Kết quả cuối khóa

## Feature

Feature này nhập điểm/kết quả cuối khóa, lọc học viên theo trạng thái kết quả và gửi email kết quả cho phụ huynh/học viên.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Nhập điểm: `lophoc!nhapdiem`
- Lọc kết quả: `lophoc!Loc_ketqua`
- Permission chính: `03CKGK`
- `Loc_ketqua.ascx.cs` dùng permission key `03CKGK ` có khoảng trắng dư.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/LOPHOC/Nhapdiem.ascx.cs` | Màn hình nhập điểm/kết quả cuối khóa, gửi email. |
| `MODULE/LOPHOC/Loc_ketqua.ascx.cs` | Lọc danh sách học viên theo trạng thái kết quả cuối khóa. |
| `MODULE/LOPHOC/Ketqua/ketqua_style1/INSERT/Addnew_auto.ascx.cs` | Form động nhập kết quả style 1. |
| `MODULE/LOPHOC/Ketqua/ketqua_style2/INSERT/Addnew_auto.ascx.cs` | Form động nhập kết quả style 2. |
| `MODULE/LOPHOC/up_diemcuoikhoa.ascx.cs` | Upload/cập nhật điểm cuối khóa, cần rà sâu riêng nếu sửa. |
| `MODULE/LOPHOC/Ketqua/*/Print/*` | In kết quả cuối khóa. |

## Logic chính

`Nhapdiem.ascx.cs` xử lý các command:

- `CP_time`: lấy khoảng ngày/lớp.
- `Load_bangdiem`: load bảng điểm.
- `SaveList_bangdiem`: lưu điểm vào `BangDiem`.
- `Update_sendemail`: cập nhật trạng thái gửi email.
- `save`: lưu dữ liệu phụ trợ.

Khi load danh sách, code dùng `ChuongTrinh.stylediem_bydangky(iddangky)` để lấy `modulediem`, từ đó trỏ sang bảng động như `Ketqua_style1`.

Form `Ketqua_style*/INSERT/Addnew_auto.ascx.cs` dùng `Form_auto` để insert/update kết quả theo:

- `idhocsinh`
- `iddangky`
- `type`

## Gửi email kết quả

`Nhapdiem.ascx.cs` có flow `Sendemail()`:

1. Đọc dữ liệu từ `Ketqua_style1`.
2. Build HTML email.
3. Gửi tới email phụ huynh/học viên.
4. Tăng/cập nhật số lần gửi `emailSend`.

Điểm cần chú ý: flow email đang gắn khá chặt với `Ketqua_style1`, nên nếu chương trình dùng style khác cần kiểm tra lại.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `BangDiem` | Bảng điểm cố định theo lớp/học viên/category. |
| `Ketqua_style1`, `Ketqua_style2` | Bảng động kết quả cuối khóa theo chương trình. |
| `Lophoc_join` | Danh sách học viên trong lớp. |
| `Hocsinh` | Hồ sơ học viên/email. |
| `Dangky` | Đăng ký khóa học. |
| `Khoahoc` | Khóa học liên quan. |
| `ChuongTrinh` | Chọn style/bảng kết quả. |

## Tương tác với module khác

- `ChuongTrinh` quyết định form/bảng kết quả.
- `HOCVIEN` cung cấp hồ sơ, email và trạng thái đăng ký.
- `LOPHOC` cung cấp lớp và học viên trong lớp.
- In ấn và email dùng dữ liệu kết quả cuối khóa để xuất ra cho phụ huynh/học viên.

## Overlap/xung đột

- `BangDiem` và `Ketqua_style*` cùng liên quan điểm/kết quả; cần xác định bảng nào dùng cho báo cáo chính.
- `Nhapdiem.ascx.cs` và `Loc_ketqua.ascx.cs` có cách xác định học viên kết thúc khóa chưa thống nhất hoàn toàn.
- Email hiện đọc style cụ thể, không rõ có hỗ trợ đầy đủ style khác.

## Vấn đề cần sửa

- **SQL injection do truy vấn SQL nối chuỗi trực tiếp**.
- Cần chuẩn hóa source of truth giữa `BangDiem` và `Ketqua_style*`.
- Cần kiểm tra lại permission key có khoảng trắng dư.
- Cần tách build HTML email và gửi email ra service riêng để dễ test.
- Cần có log/trạng thái gửi email rõ ràng hơn để tránh gửi trùng hoặc mất dấu lỗi gửi.
