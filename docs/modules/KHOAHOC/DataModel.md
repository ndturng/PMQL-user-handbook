# Data model module Khóa học

## Bảng chính

### `Chuongtrinh`

Chương trình cha của nhiều khóa học.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính. |
| `ten` | Tên chương trình. |
| `mota`/`diengiai` | Mô tả/diễn giải. |
| `logo` | Ảnh chương trình. |
| `enable` | Trạng thái còn dùng. |
| `stt` | Thứ tự hiển thị. |
| `iduser` | User tạo. |
| `idchinhanh` | Chi nhánh gốc trong một số flow cũ. |
| `style_diemdanh` | Style/module điểm danh dùng bởi `LOPHOC`. |
| `style_diem` | Style/module kết quả cuối khóa dùng bởi `LOPHOC`. |

### `Khoahoc`

Khóa học con thuộc một chương trình.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính. |
| `idchuongtrinh` | Chương trình cha. |
| `ten` | Tên khóa học. |
| `makh` | Mã khóa học. |
| `capdo` | Cấp độ/level. |
| `HP_full` | Học phí gốc. |
| `sogio` | Số giờ/số buổi gốc. |
| `thoiluong` | Thời lượng trong một số màn cũ. |
| `enable` | Trạng thái còn dùng. |
| `status` | Trạng thái hiển thị/new/active trong UI. |
| `isPublic` | Public hay dành cho người hướng dẫn. |
| `DisplayOrder` | Thứ tự hiển thị. |
| `nextCourse` | Danh sách khóa tiếp theo dạng CSV. |
| `iduser` | User tạo. |
| `idchinhanh` | Chi nhánh gốc trong một số màn cũ. |

### `KhoaHoc_ChiNhanh`

Join khóa học với chi nhánh và override thông tin thương mại.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính join. |
| `idchiNhanh` | Chi nhánh áp dụng. |
| `idchuongtrinh` | Chương trình. |
| `idkhoahoc` | Khóa học. |
| `HP_full` | Học phí theo chi nhánh. |
| `sogio` | Số giờ/số buổi theo chi nhánh. |
| `tkonline` | Phí tài khoản online. |
| `tktestdauvao` | Phí test đầu vào. |
| `iduser` | User tạo. |
| `updatetime` | Thời điểm tạo/cập nhật. |

### `Khoahoc_kho`

Join khóa học với vật tư/tài liệu đi kèm.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính. |
| `idkhoahoc` | Khóa học. |
| `idtaisan` | Vật tư/tài liệu. |
| `soluong` | Số lượng. |
| `iduser` | User tạo. |
| `updatetime` | Thời điểm tạo. |

## Bảng ngoài module nhưng phụ thuộc mạnh

| Bảng | Vai trò |
| --- | --- |
| `Dangky_group` | Phiếu đăng ký học viên theo chi nhánh. |
| `Dangky` | Dòng đăng ký khóa học, tham chiếu `IDkhoahoc`. |
| `Lophoc_join` | Xếp học viên vào lớp theo `idkhoahoc`/`iddangky`. |
| `Lophoc` | Lớp học; một số flow cũ dùng `Lophoc.idkhoahoc`. |
| `Style_format` | Module/style điểm danh/kết quả của chương trình. |
| `Donhang_setting` | Danh mục vật tư để gắn với khóa học. |
| `KHO_Chitietphieu`, `KHO` | Vật tư/tồn kho khi thanh toán đăng ký. |
| `EventTarget` | Event target theo khóa học qua danh sách course id. |

## Quan hệ dữ liệu chính

```text
Chuongtrinh
  ├─ Khoahoc
  │   ├─ KhoaHoc_ChiNhanh -> ChiNhanh
  │   ├─ Khoahoc_kho -> Donhang_setting / taisan
  │   ├─ Dangky -> Dangky_group -> Hocsinh
  │   └─ Lophoc_join -> Lophoc
  ├─ Style_format(style_diemdanh)
  └─ Style_format(style_diem)
```

## Luồng dữ liệu chính

1. Admin tạo `Chuongtrinh`.
2. Admin tạo `Khoahoc` thuộc chương trình.
3. Admin phân bổ khóa học sang chi nhánh bằng `KhoaHoc_ChiNhanh`.
4. Học viên đăng ký khóa học trong `HOCVIEN`, tạo `Dangky_group` và `Dangky`.
5. Thu học phí trong `HOCVIEN/KETOAN` dựa trên đăng ký và giá khóa học.
6. Xếp lớp trong `LOPHOC` tạo/cập nhật `Lophoc_join`.
7. Điểm danh/kết quả trong `LOPHOC` đọc style từ `Chuongtrinh`.
8. Event có thể dùng `Khoahoc` để lọc học viên đủ điều kiện.

## Issue DB cần chú ý

- `Khoahoc.HP_full` và `KhoaHoc_ChiNhanh.HP_full` cùng là học phí nhưng khác phạm vi.
- `Khoahoc.nextCourse` có vẻ là CSV, khó enforce quan hệ khóa tiếp theo.
- `Chuongtrinh.style_diemdanh/style_diem` quyết định bảng động của `LOPHOC`; nếu cấu hình sai sẽ lỗi điểm danh/kết quả.
- `Khoahoc_kho` chưa thấy unique constraint rõ cho `idkhoahoc + idtaisan`.
- Một số flow cũ dùng `Dangky.idlop`, flow mới dùng `Lophoc_join`; cần tránh suy luận nhầm source of truth.
- Nên kiểm tra index cho:
  - `Khoahoc(idchuongtrinh, capdo, enable, status)`
  - `KhoaHoc_ChiNhanh(idchiNhanh, idkhoahoc)`
  - `Dangky(IDkhoahoc, enable, status, updatetime)`
  - `Lophoc_join(idkhoahoc, iddangky, updatetime)`
  - `Khoahoc_kho(idkhoahoc, idtaisan)`
