# Data model module Lớp học

## Nhóm bảng chính

| Bảng | Vai trò | Ghi chú |
| --- | --- | --- |
| `Lophoc` | Bảng lớp học chính. | Lưu tên lớp, chi nhánh, ngày bắt đầu/kết thúc, trạng thái `enable`. |
| `Lophoc_join` | Quan hệ học viên/đăng ký với lớp. | Là bảng trung tâm cho sĩ số, điểm danh, điểm cuối khóa. |
| `Lop_TKB` | Thời khóa biểu lớp. | Mỗi dòng thường là một weekday/khung giờ/phòng/giáo viên. |
| `PhongHoc` | Danh mục phòng học. | Dùng trong `Lop_TKB`. |
| `Nhan_su` | Giáo viên/nhân sự. | `Lophoc_TKB` lọc giáo viên theo chi nhánh và vị trí. |
| `TKB_Risk` | Ngày nghỉ/ngày rủi ro. | Ảnh hưởng cách tính ngày học/ngày kết thúc. |

## Nhóm bảng học viên/đăng ký

| Bảng | Vai trò |
| --- | --- |
| `Hocsinh` | Hồ sơ học viên. |
| `Dangky` | Đăng ký khóa học. |
| `Khoahoc` | Khóa học của đăng ký. |
| `Giahan_khoahoc` | Gia hạn thời gian học. |
| `Baoluu_khoahoc` | Bảo lưu khóa học. |

## Nhóm bảng điểm danh/kết quả

| Bảng | Vai trò | Ghi chú |
| --- | --- | --- |
| `Lophoc_diemdanh` | Điểm danh cố định theo lớp/học viên/ngày. | Được `Diemdanh.ascx.cs` upsert trong `SaveList`. |
| `Diemdanh_style1`, `Diemdanh_style2` | Điểm danh/kết quả ngày dạng động. | Chọn qua `ChuongTrinh.modulediemdanh`. |
| `BangDiem` | Điểm cố định theo lớp/học viên/category. | Được `Nhapdiem.ascx.cs` lưu. |
| `Ketqua_style1`, `Ketqua_style2` | Kết quả cuối khóa dạng động. | Chọn qua `ChuongTrinh.modulediem`. |
| `ChuongTrinh` | Cấu hình chương trình. | Quyết định bảng động điểm danh/kết quả. |

## Quan hệ dữ liệu chính

```text
Lophoc
  ├─ Lop_TKB -> PhongHoc
  ├─ Lop_TKB -> Nhan_su
  └─ Lophoc_join -> Hocsinh
                  -> Dangky -> Khoahoc
                  -> Giahan_khoahoc
                  -> Baoluu_khoahoc

Dangky -> ChuongTrinh
       -> Diemdanh_style*
       -> Ketqua_style*
```

## Luồng dữ liệu chính

1. `HOCVIEN` tạo hồ sơ học viên và đăng ký khóa học.
2. Học viên/đăng ký được đưa vào lớp qua `Lophoc_join`.
3. `LOPHOC` cấu hình lớp và lịch học trong `Lop_TKB`.
4. Điểm danh/kết quả ngày đọc học viên từ `Lophoc_join`, ngày học từ `Lop_TKB`, style từ `ChuongTrinh`.
5. Kết quả cuối khóa đọc học viên từ `Lophoc_join`, style từ `ChuongTrinh`, rồi lưu vào `BangDiem` hoặc `Ketqua_style*`.
6. Thống kê lớp dùng `Lophoc`, `Lophoc_join`, ngày nghỉ/gia hạn/bảo lưu để tính sĩ số và trạng thái.

## Issue DB cần chú ý

- Chưa thấy source of truth rõ ràng giữa bảng cố định và bảng động:
  - `Lophoc_diemdanh` vs `Diemdanh_style*`
  - `BangDiem` vs `Ketqua_style*`
- `Lophoc_join` là bảng rất quan trọng nhưng có nơi hard delete, có thể mất lịch sử.
- `Lop_TKB` được cập nhật bằng delete toàn bộ rồi insert lại, cần transaction.
- Nên có constraint/index cho các truy vấn thường xuyên:
  - `Lophoc_join(idlop, idhocsinh, iddangky, fromdate, todate)`
  - `Lop_TKB(idlop, idweek, fromdate, todate)`
  - `TKB_Risk(idchinhanh, dateoff)`
  - `Ketqua_style*(idhocsinh, iddangky, type)`
  - `Diemdanh_style*(idhocsinh, iddangky, ngay)`
