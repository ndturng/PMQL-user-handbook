# HOCVIEN: Đăng ký khóa học

## Mục đích

Feature đăng ký khóa học quản lý phiếu đăng ký, danh sách khóa học trong phiếu, học phí, khoản thu khác và vật tư đi kèm.

Entry chính:

```text
Default.aspx?mod=hocvien!dangky_group&idhocvien={id}
```

File chính:

- `MODULE/HOCVIEN/Dangky_group.ascx`
- `MODULE/HOCVIEN/Dangky_group.ascx.cs`
- `MODULE/HOCVIEN/DANGKY/dangky_togroup.aspx`
- `MODULE/HOCVIEN/DANGKY/dangky_togroup.aspx.cs`
- `MODULE/HOCVIEN/DANGKY/dangky_kho.aspx.cs`
- `MODULE/HOCVIEN/DANGKY/dangky_thukhac.aspx.cs`
- `MODULE/HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs`
- `MODULE/HOCVIEN/History-registry.ascx.cs`

## Trạng thái sử dụng trong app

Đây là flow chính khi user bấm "Đăng ký khóa học" từ danh sách học viên.

Quyền chính:

```text
01DKKH
```

## Luồng phiếu đăng ký group

1. User vào `hocvien!dangky_group&idhocvien={id}`.
2. `Dangky_group.ascx.cs` kiểm tra quyền `01DKKH`, action `1`.
3. Nếu chưa có `idedit`, script gọi `Add_khoahoc()`.
4. Các khóa học được thêm vào một phiếu `Dangky_group`.
5. Mỗi khóa cụ thể nằm trong bảng `Dangky`.
6. Các khoản thu khác nằm trong `Dangky_thukhac`.
7. Vật tư đi kèm khóa học có thể được ghi vào `KHO_Chitietphieu`.

## Commands trong `Dangky_group.ascx.cs`

| Command | Function | Vai trò |
| --- | --- | --- |
| `create_group` | `Create_phieu()` | Tạo phiếu đăng ký group |
| `Load_listdangky` | `Load_listdangky()` | Load các khóa trong phiếu |
| `Update_ghichu` | `Update_ghichu()` | Sửa ghi chú phiếu |
| `Del_listdangky` | `Del_listdangky()` | Xóa một dòng đăng ký khóa |
| `Del_khodangky` | `Del_khodangky()` | Xóa vật tư đi kèm phiếu |
| `Del_thukhac` | `Del_thukhac()` | Xóa khoản thu khác chưa thu |
| `DEL_Phieudangky` | `Del_phieudangky()` | Xóa phiếu đăng ký group nếu chưa phát sinh học phí |

## Luồng thêm khóa vào phiếu

`DANGKY/dangky_togroup.aspx.cs` xử lý:

| Command | Vai trò |
| --- | --- |
| `loadchuongtrinh` | Load chương trình theo chi nhánh |
| `loadcapdo` | Load cấp độ theo chương trình/học viên |
| `loadkhoahoc` | Load khóa học phù hợp |
| `load_lophoc` | Load lớp học theo chi nhánh |
| `khuyenmai` / `load_khuyenmai` | Load khuyến mãi |
| `kho_follow` | Load vật tư/kho đi kèm |
| `insert_dangky` | Insert dòng `Dangky` |

Khi insert đăng ký khóa:

1. Lấy `IDhocsinh` từ `Dangky_group`.
2. Check trùng khóa trong cùng phiếu bằng `Check_replace_regis(...)`.
3. Insert `Dangky`.
4. Nếu chọn tài khoản online, insert `Dangky_thukhac`.
5. Nếu chọn vật tư theo khóa, insert `KHO_Chitietphieu`.

## Lịch sử đăng ký

`History-registry.ascx.cs` load `Dangky_group` theo chi nhánh, học viên, ngày đăng ký. Nó tính học phí đã thu từ:

- `Hocphi` qua `Sum_hocphi_dathu(...)`.
- Nếu không có, fallback sang `ThuChi` qua `Sum_hocphi_dathu_new(...)`.

Điều này cho thấy hệ thống có hai nguồn dữ liệu tài chính cũ/mới cần đọc song song.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Dangky_group` | Phiếu đăng ký tổng |
| `Dangky` | Dòng đăng ký từng khóa học |
| `Khoahoc` | Khóa học đăng ký |
| `Chuongtrinh` | Chương trình của khóa |
| `Lophoc` | Lớp gán cho khóa |
| `Dangky_thukhac` | Khoản thu khác |
| `Hocphi` | Học phí legacy |
| `ThuChi` | Phiếu thu mới |
| `KhuyenMai` | Khuyến mãi |
| `KHO_Chitietphieu` | Vật tư đi kèm đăng ký |
| `Khoahoc_kho`, `donhang_setting` | Cấu hình vật tư theo khóa |

## Vấn đề cần chú ý

- Xóa phiếu đăng ký group xóa nhiều bảng (`Dangky_group`, `Dangky`, `Taisan_Nhap_Xuat`, `Dangky_thukhac`) nhưng **không thấy transaction**.
- `Del_listdangky()` xóa cứng dòng `Dangky`, không soft-delete.
- `Del_thukhac()` chỉ xóa khi `status=0`, đây là guard đúng cần giữ.
- Có nhiều file đăng ký tương tự ở root HOCVIEN và trong `DANGKY/`, cần xác định URL thực tế trước khi sửa.
- Học phí đã thu được đọc từ cả `Hocphi` và `ThuChi`, nên sửa báo cáo/hủy phiếu phải kiểm tra cả hai.
- **SQL injection do truy vấn SQL nối chuỗi trực tiếp** rất nhiều từ request/form.
- Vật tư kho được insert khi đăng ký nhưng đoạn kiểm tồn kho đang bị comment trong `dangky_togroup.aspx.cs`; cần xác nhận nghiệp vụ trước khi bật lại.
