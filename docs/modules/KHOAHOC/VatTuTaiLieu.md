# Vật tư và tài liệu đi kèm khóa học

## Feature

Feature này gắn vật tư/tài liệu đi kèm một khóa học. Dữ liệu này ảnh hưởng flow đăng ký học viên và kho/vật tư khi thanh toán.

## Trạng thái trong flow chính

Có dùng trong flow quản lý khóa học và có tác động tới đăng ký/học phí.

- Entry từ danh sách khóa học: nút `Tài Liệu`.
- File chính: `MODULE/KHOAHOC/Khoahoc_vattu.aspx.cs`.
- Permission đang check: `01DKKH`, action `2`.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/KHOAHOC/Khoahoc_vattu.aspx.cs` | Gắn/xóa/load vật tư đi kèm khóa học. |
| `MODULE/KHOAHOC/Khoahoc_kho.aspx.cs` | Có route insert từ UI vật tư, cần rà sâu nếu sửa flow. |
| `MODULE/HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs` | Khi thu học phí, cập nhật trạng thái vật tư/tồn kho theo phiếu đăng ký. |
| `MODULE/KHO/`, `MODULE/KHOVATTU/`, `MODULE/QuanLyKho/` | Các module kho/vật tư liên quan dữ liệu tồn. |

## Logic chính

`Khoahoc_vattu.aspx.cs` xử lý:

- `loadvattu`: load vật tư từ `Donhang_setting` với `type='1'` và `enable='1'`.
- `insert_dangky`: insert vật tư vào `Khoahoc_kho`.
- `Load_added`: load danh sách vật tư đã gắn với khóa học.
- `DEL_kh_kho`: delete vật tư đã gắn khỏi `Khoahoc_kho`.

Khi thêm vật tư:

1. Nhận `idtaisan` từ field tên `idkhoahoc` trong form.
2. Nhận `soluong`.
3. Nhận khóa học từ `idlenh_formkho`.
4. Kiểm tra vật tư đã gắn với khóa học chưa.
5. Insert `Khoahoc_kho(iduser, idkhoahoc, idtaisan, soluong, updatetime)`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Khoahoc_kho` | Bảng join khóa học với vật tư/tài liệu. |
| `Donhang_setting` | Danh mục vật tư đang được chọn trong `loadvattu`. |
| `taisan` | Một hàm cũ `Load_khofllow()` vẫn query bảng này. |
| `KHO_Chitietphieu`, `KHO` | Bị cập nhật khi thanh toán đăng ký học viên có vật tư. |

## Tương tác với module khác

- `HOCVIEN` khi tạo/thu phiếu đăng ký có thể đưa vật tư theo khóa học vào đơn/phiếu.
- `KETOAN` ghi nhận học phí và vật tư trong phiếu đăng ký.
- `KHO` bị trừ tồn khi thu học phí thành công trong `Phieuthu_hocphi.aspx.cs`.

## Overlap/xung đột

- Code dùng cả `Donhang_setting` và `taisan` trong các đoạn khác nhau; cần xác định bảng nào là source hiện tại.
- Field form tên `idkhoahoc` nhưng thực tế chứa `idtaisan`, dễ gây nhầm.
- Xóa vật tư khỏi `Khoahoc_kho` là hard delete.

## Vấn đề cần sửa

- Sửa lỗi SQL injection: thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query.
- Chuẩn hóa naming: `idtaisan` không nên gửi qua field `idkhoahoc`.
- Không hard delete nếu vật tư đã được dùng trong đăng ký/phiếu kho.
- Cần rõ rule khi thay đổi vật tư đi kèm: có áp dụng retroactive cho đăng ký cũ hay chỉ đăng ký mới.
- Cần thống nhất danh mục vật tư hiện tại là `Donhang_setting` hay `taisan`.
