# Phân bổ khóa học theo chi nhánh

## Feature

Feature này cấu hình khóa học nào được bán/dùng ở chi nhánh nào, đồng thời override học phí, số buổi và phí test/online theo chi nhánh.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Entry: `Default.aspx?mod=khoahoc!khoahoc_chinhanh`
- Permission chính: `Users.Check_QuanLy()`
- Xóa join cần `02PQ`, action `4`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/KHOAHOC/khoahoc_chinhanh.ascx.cs` | Load danh sách khóa học theo chi nhánh. |
| `MODULE/KHOAHOC/Khoahoc_toCN.aspx.cs` | Thêm khóa học vào chi nhánh. |
| `MODULE/KHOAHOC/Khoahoc_toCN_edit.ascx.cs` | Sửa cấu hình học phí/số buổi/phí theo chi nhánh. |
| `App_Code/BO/ChuongTrinh.cs` | Helper `Chuongtrinh_byChinhanh`, `khoahoc_byChinhanh`. |

## Logic danh sách

`khoahoc_chinhanh.ascx.cs`:

1. Check `Users.Check_QuanLy()`.
2. Load `Chuongtrinh.enable=1`.
3. Với từng chương trình, load các cấp độ có khóa học trong `KhoaHoc_ChiNhanh` theo `idchinhanh`.
4. Load các khóa học join với `KhoaHoc_ChiNhanh`.
5. Render:
   - tên khóa học
   - mã khóa học
   - học phí theo chi nhánh `KhoaHoc_ChiNhanh.HP_full`
   - số giờ/số buổi `KhoaHoc_ChiNhanh.sogio`
   - phí tài khoản online `tkonline`
   - phí test đầu vào `tktestdauvao`
   - nút sửa/xóa join.

## Logic thêm vào chi nhánh

`Khoahoc_toCN.aspx.cs`:

1. Load chương trình.
2. Load cấp độ theo chương trình.
3. Load khóa học theo chương trình/cấp độ.
4. Khi submit `insert_dangky`:
   - kiểm tra khóa học đã có trong chi nhánh chưa.
   - insert `KhoaHoc_ChiNhanh` với `iduser`, `idchiNhanh`, `idchuongtrinh`, `idkhoahoc`, `HP_full`, `sogio`, `tkonline`, `tktestdauvao`.

## Logic sửa cấu hình chi nhánh

`Khoahoc_toCN_edit.ascx.cs`:

- Load tên khóa học, học phí gốc từ `Khoahoc`, học phí/số buổi/phí theo chi nhánh từ `KhoaHoc_ChiNhanh`.
- `cmd=save` update:

```text
HP_full
tktestdauvao
tkonline
sogio
```

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `KhoaHoc_ChiNhanh` | Bảng join khóa học với chi nhánh và giá/số buổi theo chi nhánh. |
| `Khoahoc` | Khóa học gốc. |
| `Chuongtrinh` | Chương trình cha. |
| `ChiNhanh` | Chi nhánh áp dụng. |

## Tương tác với module khác

- `HOCVIEN` khi đăng ký khóa học thường cần danh sách khóa học theo chi nhánh.
- `KETOAN` thu học phí dựa trên giá khóa học/phiếu đăng ký.
- `Event` lọc học viên theo `Khoahoc`, nhưng cần lưu ý event load `Khoahoc` global ở vài đoạn, không nhất thiết qua `KhoaHoc_ChiNhanh`.
- `LOPHOC` và thống kê lớp cần biết khóa học thuộc chi nhánh nào để tính đúng danh sách.

## Overlap/xung đột

- `Khoahoc.HP_full/sogio` là giá/số buổi gốc, còn `KhoaHoc_ChiNhanh.HP_full/sogio` là override theo chi nhánh. Cần biết màn nào đọc bảng nào.
- Xóa join `KhoaHoc_ChiNhanh` là hard delete, có thể ảnh hưởng dữ liệu đăng ký nếu đăng ký từng dùng cấu hình đó.
- Check trùng khóa học trong chi nhánh làm ở code, chưa thấy unique constraint.

## Vấn đề cần sửa

- Sửa lỗi SQL injection: thay truy vấn SQL nối chuỗi trực tiếp bằng parameterized query.
- Thêm unique constraint/index cho `KhoaHoc_ChiNhanh(idchiNhanh, idkhoahoc)`.
- Không nên hard delete join nếu đã có đăng ký sử dụng khóa học ở chi nhánh đó.
- Cần chuẩn hóa rule: khi sửa giá theo chi nhánh, có ảnh hưởng đăng ký đã tạo hay chỉ đăng ký mới.
