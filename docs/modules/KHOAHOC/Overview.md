# Module Khóa học (`MODULE/KHOAHOC`)

## Mục đích

`MODULE/KHOAHOC` quản lý danh mục chương trình/khóa học và các cấu hình để khóa học được sử dụng trong đăng ký học viên, lớp học, điểm danh/kết quả, event và kế toán.

Các nhóm chính:

- Quản lý `Chuongtrinh`.
- Quản lý `Khoahoc`.
- Đổi/tổ chức cấp độ khóa học.
- Phân bổ khóa học theo chi nhánh qua `KhoaHoc_ChiNhanh`.
- Gắn vật tư/tài liệu đi kèm khóa học.
- Thống kê đăng ký khóa học.
- Một số màn hình lớp/chờ lớp cũ có overlap với `LOPHOC` và `HOCVIEN`.

Entry chính:

```text
Default.aspx?mod=khoahoc!home
```

## Trạng thái sử dụng trong app

Module này đang nằm trong flow chính của app.

Bằng chứng:

- `MODULE/menu/menu_left.ascx` trỏ menu khóa học tới `Default.aspx?mod=khoahoc!home`.
- `MODULE/KHOAHOC/home.ascx` có entry quản lý chương trình/khóa học, chương trình theo cơ sở, thống kê đăng ký.
- `HOCVIEN` dùng `Khoahoc`/`Chuongtrinh` khi đăng ký khóa học, thu học phí, chuyển khóa/lớp, bảo lưu.
- `LOPHOC` dùng `ChuongTrinh.chuongtrinh_bydangky()` và `stylediem_bydangky()` để quyết định style điểm danh/kết quả.
- `Event` dùng `Khoahoc` để lọc học viên đủ điều kiện tham gia sự kiện.
- `KETOAN` dùng khóa học/đăng ký để tính học phí và báo cáo.

## Tài liệu con

- [DanhMuc.md](./DanhMuc.md): chương trình, khóa học, cấp độ, form thêm/sửa.
- [PhanBoChiNhanh.md](./PhanBoChiNhanh.md): khóa học theo chi nhánh và override học phí/số buổi/phí test.
- [VatTuTaiLieu.md](./VatTuTaiLieu.md): vật tư/tài liệu đi kèm khóa học.
- [ThongKeDangKy.md](./ThongKeDangKy.md): thống kê đăng ký/chờ lớp/vào lớp/hủy.
- [DataModel.md](./DataModel.md): bảng dữ liệu và quan hệ.
- [Issues.md](./Issues.md): overlap, rủi ro logic, DB, bảo mật, hiệu suất và việc cần sửa.

## File liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/KHOAHOC/home.ascx` | Dashboard module. |
| `MODULE/KHOAHOC/Menu_top.ascx` | Menu con module. |
| `MODULE/KHOAHOC/Danhsach.ascx.cs` | Danh sách chương trình/khóa học, xóa chương trình/khóa học. |
| `MODULE/KHOAHOC/Chuongtrinh.ascx.cs` | Danh sách chương trình dạng riêng. |
| `MODULE/KHOAHOC/Chuongtrinh_Addnew_auto.ascx.cs` | Form động thêm/sửa chương trình. |
| `MODULE/KHOAHOC/Khoahoc_Addnew_auto.ascx.cs` | Form động thêm/sửa khóa học. |
| `MODULE/KHOAHOC/Change_capdo.ascx.cs` | Đổi tên cấp độ theo chương trình. |
| `MODULE/KHOAHOC/khoahoc_chinhanh.ascx.cs` | Danh sách khóa học theo chi nhánh. |
| `MODULE/KHOAHOC/Khoahoc_toCN.aspx.cs` | Thêm khóa học vào chi nhánh. |
| `MODULE/KHOAHOC/Khoahoc_toCN_edit.ascx.cs` | Sửa học phí/số buổi/phí test theo chi nhánh. |
| `MODULE/KHOAHOC/Khoahoc_vattu.aspx.cs` | Gắn vật tư/tài liệu với khóa học. |
| `MODULE/KHOAHOC/thongke_dangky.ascx.cs` | Thống kê đăng ký khóa học. |
| `MODULE/KHOAHOC/Wating-khoahoc.ascx.cs` | Màn chờ lớp cũ theo khóa học. |
| `MODULE/KHOAHOC/List_class*.ascx.cs` | Màn lớp hiện tại/kết thúc cũ, overlap `LOPHOC`. |
| `MODULE/KHOAHOC/SelectTo_Class.ascx.cs` | Gán/gỡ đăng ký vào lớp bằng `Dangky.idlop`, overlap flow cũ. |
| `App_Code/BO/khoahoc.cs` | Helper `KHoahoc`. |
| `App_Code/BO/ChuongTrinh.cs` | Helper `ChuongTrinh`, dùng bởi `LOPHOC`, `HOCVIEN`, `Event`. |

## Feature map

| Feature | Entry/command | File chính | Bảng chính |
| --- | --- | --- | --- |
| Dashboard | `khoahoc!home` | `home.ascx` | Không trực tiếp |
| Danh sách chương trình/khóa học | `cmd=Load_khoahoc` | `Danhsach.ascx.cs` | `Chuongtrinh`, `Khoahoc` |
| Thêm/sửa chương trình | form auto | `Chuongtrinh_Addnew_auto.ascx.cs` | `Chuongtrinh` |
| Thêm/sửa khóa học | form auto | `Khoahoc_Addnew_auto.ascx.cs` | `Khoahoc` |
| Xóa chương trình | `cmd=del_chuongtrinh` | `Danhsach.ascx.cs`, `Chuongtrinh.ascx.cs` | `Chuongtrinh` |
| Xóa khóa học | `cmd=del_khoahoc` | `Danhsach.ascx.cs`, `KHoahoc.Delkhoahoc()` | `Khoahoc`, `Dangky` |
| Đổi cấp độ | post form | `Change_capdo.ascx.cs` | `Khoahoc.capdo` |
| Khóa học theo chi nhánh | `cmd=Load_khoahoc` | `khoahoc_chinhanh.ascx.cs` | `KhoaHoc_ChiNhanh` |
| Thêm vào chi nhánh | `cmd=insert_dangky` | `Khoahoc_toCN.aspx.cs` | `KhoaHoc_ChiNhanh` |
| Sửa cấu hình chi nhánh | `cmd=save` | `Khoahoc_toCN_edit.ascx.cs` | `KhoaHoc_ChiNhanh` |
| Vật tư/tài liệu đi kèm | `loadvattu`, `insert_dangky`, `Load_added` | `Khoahoc_vattu.aspx.cs` | `Khoahoc_kho`, `Donhang_setting` |
| Thống kê đăng ký | `statics_regis`, `loadCT`, `loadKH` | `thongke_dangky.ascx.cs` | `Dangky`, `Dangky_group`, `Lophoc_join` |

## Permission key chính

| Permission | Ý nghĩa quan sát được |
| --- | --- |
| `21QLKH` | Entry quản lý chương trình/khóa học trên home/menu. |
| `02QKDM` | Quyền thao tác danh mục khóa học/chương trình trong code. |
| `02PQ` | Xóa khóa học khỏi chi nhánh. |
| `02TK` | Thống kê đăng ký khóa học. |
| `01DKKH` | Flow gắn vật tư/tài liệu đi kèm khóa học. |
| `Users.Check_QuanLy()` | Quyền quản lý cho phân bổ khóa học theo chi nhánh. |

## Nhận xét nhanh

`KHOAHOC` là module cấu hình nền, không chỉ là danh mục. `Chuongtrinh` quyết định style điểm danh/kết quả; `Khoahoc` quyết định học phí, thời lượng, cấp độ, course visibility; `KhoaHoc_ChiNhanh` override dữ liệu theo cơ sở. Sửa module này có thể ảnh hưởng trực tiếp đến đăng ký học viên, lớp học, thu học phí, event và báo cáo.
