# HOCVIEN: Bảo lưu

## Mục đích

Nhóm bảo lưu xử lý hai loại nghiệp vụ dễ bị nhầm:

- Bảo lưu học phí/voucher cho học viên.
- Bảo lưu hoặc tạm dừng khóa học đang học, ảnh hưởng `Dangky` và `Lophoc_join`.

File chính:

- `MODULE/HOCVIEN/Baoluu_hocphi.ascx.cs`
- `MODULE/HOCVIEN/Baoluu_kq.ascx.cs`
- `MODULE/HOCVIEN/Baoluu_kqView.ascx.cs`
- `MODULE/HOCVIEN/Thongke_baoluu.ascx.cs`
- `MODULE/HOCVIEN/Danhsach.ascx.cs`

## Trạng thái sử dụng trong app

Dashboard `home.ascx` có entry:

```text
Default.aspx?mod=hocvien!thongke_baoluu
```

`Danhsach.ascx.cs` cũng có các command trực tiếp thao tác trạng thái bảo lưu/gia hạn/kết thúc sớm.

## Bảo lưu học phí

`Baoluu_hocphi.ascx.cs` dùng bảng:

```text
Hocsinh_voucher
```

Luồng thêm mới:

1. Mở form với `idhv` và `iddk`.
2. Kiểm tra quyền `01NTK`, action `2`.
3. Dùng `Form_auto` để build insert vào `Hocsinh_voucher`.
4. Ghi log vào `Nhansu_Log` với nội dung bảo lưu học phí.

## Bảo lưu/khôi phục khóa học

Một phần logic nằm trong `Danhsach.ascx.cs`:

| Function | Tác động |
| --- | --- |
| `Submit_reactive()` | Kích hoạt lại khóa bảo lưu; update `Lophoc_join`, `Dangky`, `Baoluu_khoahoc` |
| `Submit_extend()` | Gia hạn khóa học 7 ngày; insert `giahan_khoahoc` |
| `End_extend()` | Kết thúc gia hạn; update `Giahan_khoahoc`, `Lophoc_join` |
| `Submit_endsoon()` | Kết thúc sớm; set `Lophoc_join.status_end=1`, lưu `todate_temp` |
| `Update_statusEnd()` | Khôi phục kết thúc sớm; set `status_end=0` |
| `Cancel_khoahoc()` | Hủy khóa và tạo voucher từ `Tonghocphi` |

## Thống kê bảo lưu

`Thongke_baoluu.ascx.cs`:

- Kiểm tra quyền `01TK`.
- Load chi nhánh user được phép xem.
- Có command `Load_tiemnang` gọi `loadsearch()`.
- Query `Hocsinh`, `Users`, `Dangky` theo chi nhánh, trạng thái đăng ký và khoảng ngày.
- Có cơ chế chọn cột động qua `Form_auto`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Hocsinh_voucher` | Voucher/bảo lưu học phí |
| `Baoluu_khoahoc` | Bảo lưu khóa học theo `iddangky` |
| `Dangky` | Trạng thái đăng ký, status bảo lưu, ngày bảo lưu |
| `Lophoc_join` | Ngày học thực tế, kết thúc sớm, ngày tạm |
| `Giahan_khoahoc` | Lịch sử gia hạn |
| `Nhansu_Log` | Log nghiệp vụ bảo lưu |
| `Hocsinh`, `Users` | Thống kê/hiển thị |

## Vấn đề cần chú ý

- Có nhiều loại "bảo lưu" khác nhau; cần xác định đang nói về học phí/voucher hay khóa học.
- Các flow update nhiều bảng nhưng **không thấy transaction**.
- `Submit_reactive()` tính `daylefts` từ khoảng bảo lưu rồi update `Lophoc_join.todate`; cần kiểm tra kỹ công thức trước khi sửa.
- `Cancel_khoahoc()` tạo voucher bằng `Tonghocphi`, không thấy kiểm số tiền đã thu/đã dùng.
- `Thongke_baoluu.ascx.cs` có rủi ro SQL injection do truy vấn SQL nối chuỗi trực tiếp từ query string/form.
- Date filter dùng `between tungay and denngay.AddDays(1)`, cần thống nhất boundary ngày nếu sửa báo cáo.
- Tên command `Load_tiemnang` trong thống kê bảo lưu dễ gây nhầm với học viên tiềm năng.
