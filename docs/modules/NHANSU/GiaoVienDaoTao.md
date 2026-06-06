# Giáo viên, lớp và đào tạo

## Feature

Feature này mô tả các phần liên quan giáo viên: giáo viên trong thời khóa biểu, lớp đang dạy, giáo viên tham gia khóa/luyện tập online, tập huấn và chứng nhận.

## Trạng thái trong flow chính

Có dùng trong flow chính, nhưng logic phân tán nhiều module.

- `NHANSU` quản lý hồ sơ giáo viên/nhân sự.
- `LOPHOC` chọn giáo viên trong `Lop_TKB`.
- `KHOADAOTAO` quản lý khóa tập huấn/đánh giá.
- `Event` lọc giáo viên đủ điều kiện theo `Giaovien_Taphuan`.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/NHANSU/TeacherJoinClass.ascx.cs` | Danh sách giáo viên tham gia khóa/luyện tập. |
| `MODULE/NHANSU/Update_JoinClass.ascx.cs` | Thêm giáo viên vào `TeacherJoinClass`. |
| `MODULE/NHANSU/Class_bygiaovien.ascx.cs` | Xem lớp theo giáo viên. |
| `MODULE/NHANSU/List_giaovien.ascx.cs` | Danh sách giáo viên dạng cũ. |
| `MODULE/NHANSU/RATE_GV.ascx.cs` | Cập nhật `rate`, `Istaphuan` trên `Nhan_su`. |
| `MODULE/NHANSU/Add_giaovien_auto.ascx.cs` | Chứng nhận, ngày bắt đầu làm việc, form giáo viên. |
| `MODULE/LOPHOC/Lophoc_TKB.ascx.cs` | Chọn giáo viên cho từng buổi học. |
| `MODULE/Event/List.ascx.cs` | Lọc giáo viên cho event. |
| `MODULE/KHOADAOTAO/*` | Khóa tập huấn, đánh giá, chứng nhận. |

## Giáo viên trong thời khóa biểu

`LOPHOC/Lophoc_TKB.ascx.cs` load giáo viên:

```sql
select id, hoten
from Nhan_su
where enable=1
  and vitri='1'
  and temp=0
  and idchinhanh='{chi nhánh lớp}'
```

Khi lưu lịch, `Lop_TKB.idgiaovien` nhận `Nhan_su.id`.

## Giáo viên tham gia khóa/luyện tập online

`TeacherJoinClass.ascx.cs`:

- Check permission `04NS `.
- `loadlist` filter theo:
  - `idchinhanh`
  - `search`
  - `status`: `dangdoi`, `danghoc`, `ketkhoa`
- Query `TeacherJoinClass` join `Nhan_su`, `Khoahoc`, `ChiNhanh`.
- `cancel` hard delete dòng `TeacherJoinClass`.

`Update_JoinClass.ascx.cs`:

- Check permission `17KTH`, action `2`.
- Load nhân sự theo chi nhánh.
- Load khóa học `Khoahoc where isPublic=0`.
- Insert `TeacherJoinClass` với:
  - `IdNhansu`
  - `IdKhoahoc`
  - `Fromdate`
  - `Todate = Fromdate + 84 ngày`
  - `Status=1`

## Tập huấn và chứng nhận

Các bảng liên quan quan sát được:

- `Giaovien_Taphuan` / `GiaoVien_Taphuan`
- `KhoaTapHuan`
- `Danhgia_taphuan`
- `GCN_Nhansu`

`Danhsach.ascx.cs` ưu tiên đọc chứng nhận mới từ `GCN_Nhansu`, nếu không có thì fallback sang `Danhgia_taphuan` hoặc field `Nhan_su.NumberCertificate`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Nhan_su` | Hồ sơ giáo viên. |
| `Lop_TKB` | Lịch lớp tham chiếu giáo viên. |
| `TeacherJoinClass` | Giáo viên tham gia khóa/luyện tập online. |
| `Khoahoc` | Khóa/lộ trình luyện tập online. |
| `Giaovien_Taphuan` / `GiaoVien_Taphuan` | Giáo viên tham gia khóa tập huấn. |
| `KhoaTapHuan` | Khóa tập huấn. |
| `Danhgia_taphuan` | Kết quả đánh giá tập huấn. |
| `GCN_Nhansu` | Giấy chứng nhận nhân sự. |

## Tương tác với module khác

- `LOPHOC`: giáo viên được chọn trong thời khóa biểu.
- `Event`: event dành cho giáo viên lọc theo tập huấn.
- `KHOADAOTAO`: quản lý khóa tập huấn, đánh giá, chứng nhận.
- `KHOAHOC`: một số khóa `isPublic=0` được dùng cho `TeacherJoinClass`.

## Overlap/xung đột

- Có nhiều bảng/tên gần giống: `Giaovien_Taphuan` và `GiaoVien_Taphuan`.
- `TeacherJoinClass` dùng `Khoahoc`, trong khi tập huấn dùng `KhoaTapHuan`.
- `TeacherJoinClass.cancel` hard delete.
- Chuyển cơ sở trong `Exchange_nhansu` update `Lop_TKB.idgiaovien` hàng loạt sang giáo viên thay thế.

## Vấn đề cần sửa

- SQL injection do truy vấn SQL nối chuỗi trực tiếp.
- Cần transaction/log khi chuyển giáo viên trong `Lop_TKB`.
- Cần thống nhất source of truth cho chứng nhận: `GCN_Nhansu` hay `Danhgia_taphuan`.
- Cần chuẩn hóa tên bảng `Giaovien_Taphuan`/`GiaoVien_Taphuan`.
- Không nên hard delete `TeacherJoinClass`; nên soft delete/audit.
