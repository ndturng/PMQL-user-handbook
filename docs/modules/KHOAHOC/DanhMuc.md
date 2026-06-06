# Danh mục chương trình và khóa học

## Feature

Feature này quản lý cây danh mục:

```text
Chuongtrinh
  └─ Khoahoc
      └─ capdo
```

Đây là source cấu hình khóa học được các module khác sử dụng.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Entry: `Default.aspx?mod=khoahoc!danhsach`
- Permission UI home: `21QLKH`
- Permission thao tác trong code: `02QKDM`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/KHOAHOC/Danhsach.ascx.cs` | Load danh sách chương trình/khóa học, xóa chương trình/khóa học. |
| `MODULE/KHOAHOC/Chuongtrinh.ascx.cs` | Danh sách chương trình dạng riêng. |
| `MODULE/KHOAHOC/Chuongtrinh_Addnew_auto.ascx.cs` | Form động thêm/sửa chương trình. |
| `MODULE/KHOAHOC/Khoahoc_Addnew_auto.ascx.cs` | Form động thêm/sửa khóa học. |
| `MODULE/KHOAHOC/Change_capdo.ascx.cs` | Đổi tên cấp độ hàng loạt theo chương trình. |
| `MODULE/KHOAHOC/Khoahoc_settings.aspx.cs` | Cài đặt visibility/status/display order khóa học. |
| `App_Code/BO/khoahoc.cs` | Helper `KHoahoc`. |
| `App_Code/BO/ChuongTrinh.cs` | Helper `ChuongTrinh`. |

## Logic danh sách

`Danhsach.ascx.cs`:

1. Check quyền `02QKDM`.
2. `cmd=Load_khoahoc` load `Chuongtrinh.enable=1`.
3. Với mỗi chương trình, load các `capdo` đang có trong `Khoahoc`.
4. Với mỗi cấp độ, load khóa học:
   - `Khoahoc.enable=1`
   - `Khoahoc.idchuongtrinh = chương trình`
   - `Khoahoc.capdo = cấp độ`
5. Render các thông tin:
   - tên khóa học
   - mã khóa học `makh`
   - học phí `HP_full`
   - số giờ/số buổi `sogio`
   - trạng thái public/guide
   - nút cài đặt, tài liệu, sửa, xóa.

## Logic thêm/sửa

`Chuongtrinh_Addnew_auto.ascx.cs` và `Khoahoc_Addnew_auto.ascx.cs` dùng `Form_auto` để sinh form theo metadata DB.

Các điểm chính:

- Insert/update theo field từ `Form_auto.List_Column()`.
- Có upload file vào `/uploads`.
- Có permission add/edit:
  - `02QKDM`, action `2` cho thêm.
  - `02QKDM`, action `3` cho sửa.

## Logic xóa

Chương trình:

- `Danhsach.ascx.cs` set `Chuongtrinh.enable=0`.
- `ChuongTrinh.delete()` chỉ xóa mềm nếu `iduser` khớp user tạo.

Khóa học:

- `KHoahoc.Delkhoahoc(id, iduser)` kiểm tra `Dangky`.
- Nếu chưa có đăng ký, delete thẳng `Khoahoc`.
- Nếu đã có đăng ký hoặc delete không chạy, set `Khoahoc.enable=0`.

## Logic đổi cấp độ

`Change_capdo.ascx.cs`:

- Load số lượng khóa học có cùng `capdo` và `idchuongtrinh`.
- Khi submit, update hàng loạt:

```sql
update khoahoc
set capdo = N'{namenew}'
where capdo like N'{nameold}'
  and idchuongtrinh = '{idchuongtrinh}'
```

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Chuongtrinh` | Chương trình cha, có style điểm danh/kết quả. |
| `Khoahoc` | Khóa học con, học phí, thời lượng, cấp độ, trạng thái. |
| `Dangky` | Dùng kiểm tra trước khi xóa khóa học. |
| `Users` | Người tạo dữ liệu. |
| `Style_format` | Style điểm danh/kết quả qua `ChuongTrinh`. |

## Tương tác với module khác

- `HOCVIEN` dùng `Khoahoc` khi đăng ký, chuyển khóa, bảo lưu.
- `LOPHOC` dùng `ChuongTrinh` để chọn style điểm danh/kết quả theo đăng ký.
- `Event` dùng danh sách `Khoahoc` để cấu hình target event theo khóa học.
- `KETOAN` dùng `Khoahoc`/`Dangky` để tính học phí và báo cáo.

## Overlap/xung đột

- Có cả `Danhsach.ascx.cs` và `Chuongtrinh.ascx.cs` cùng quản lý chương trình.
- `Khoahoc_settings.aspx.cs` là màn cấu hình riêng chỉ một số user thấy từ danh sách.
- Một số file class có tên partial class sai domain như `MODULE_HOCVIEN_Danhsach`, dễ gây nhầm khi maintain.

## Vấn đề cần sửa

- SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Upload file chưa thấy validate extension/MIME/size.
- Xóa khóa học có thể hard delete nếu chưa có đăng ký; nên cân nhắc soft delete nhất quán.
- Đổi cấp độ update hàng loạt bằng tên text, có thể đổi nhầm nếu cấp độ trùng/không chuẩn hóa.
- Form auto có nhiều đoạn leftover cập nhật `hocsinh.mahs`, không đúng domain khóa học/chương trình.
