# Data model module Nhân sự

## Bảng chính

### `Nhan_su`

Bảng hồ sơ nhân sự/giáo viên.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính nhân sự. |
| `hoten` | Họ tên. |
| `diachi`, `dienthoai`, `email` | Thông tin liên hệ. |
| `ngaysinh`, `gioitinh` | Thông tin cá nhân. |
| `trinhdo`, `nghiepvu`, `daotao` | Thông tin trình độ/nghiệp vụ/đào tạo. |
| `vitri` | Vị trí/chức danh; `vitri='1'` thường là giáo viên. |
| `idchinhanh` | Chi nhánh. |
| `enable` | Trạng thái còn dùng. |
| `temp` | Trạng thái tạm/chính thức. |
| `isTraining` | Trạng thái đào tạo. |
| `DateStartWork` | Ngày bắt đầu làm việc. |
| `NumberCertificate` | Số chứng nhận fallback. |
| `Count_thamnien` | Cờ/tính thâm niên. |
| `rate`, `Istaphuan` | Cấu hình/rate/tập huấn giáo viên. |

### `Users`

Tài khoản đăng nhập liên kết nhân sự.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính user. |
| `idnhansu` | Nhân sự liên kết. |
| `hoten` | Tên user, sync từ nhân sự. |
| `idChinhanh` | Chi nhánh user. |
| `lever` | Level/quyền tổng. |
| `enable`, `active` | Trạng thái tài khoản. |
| `inputuser`, `inputpass` | Tài khoản/mật khẩu. |
| `DateStartWork` | Ngày bắt đầu làm việc sync từ nhân sự. |

### `GCN_Nhansu`

Giấy chứng nhận nhân sự.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `idnhansu` | Nhân sự. |
| `sogiachungnhan` | Số giấy chứng nhận. |
| `ngaycap` | Ngày cấp. |
| `ngayhethan` | Ngày hết hạn. |
| `updatetime` | Ngày cập nhật. |

### `TeacherJoinClass`

Giáo viên tham gia khóa/luyện tập online.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `Id` | Khóa chính. |
| `IdNhansu` | Giáo viên/nhân sự. |
| `IdKhoahoc` | Khóa học. |
| `Fromdate`, `Todate` | Thời gian tham gia. |
| `Status` | Trạng thái. |
| `IdKhoadaotao` | Khóa đào tạo liên quan nếu có. |
| `IdUser`, `Updatetime` | Người tạo/thời điểm. |

## Bảng đào tạo/tập huấn liên quan

| Bảng | Vai trò |
| --- | --- |
| `Giaovien_Taphuan` / `GiaoVien_Taphuan` | Giáo viên tham gia khóa tập huấn. |
| `KhoaTapHuan` | Khóa tập huấn. |
| `Danhgia_taphuan` | Đánh giá/kết quả tập huấn. |

## Bảng ngoài module nhưng phụ thuộc mạnh

| Bảng | Vai trò |
| --- | --- |
| `Lop_TKB` | Thời khóa biểu lớp, tham chiếu giáo viên qua `idgiaovien`. |
| `Khoahoc` | Khóa học cho `TeacherJoinClass`. |
| `Dangky_group` | Phiếu đăng ký để thống kê doanh thu/doanh số. |
| `Dangky_thukhac` | Thu khác trong thống kê doanh thu. |
| `Hocphi` | Học phí cũ trong thống kê doanh thu. |
| `Hocsinh` | Học viên tạo bởi user/nhân sự. |
| `Lophoc_join` | Dùng thống kê tái ký/không tái ký. |
| `Permission_join` | Quyền user liên kết nhân sự. |
| `Config_data_text` | Vị trí/cấp bậc/level. |

## Quan hệ dữ liệu chính

```text
Nhan_su
  ├─ Users
  │   ├─ Permission_join
  │   ├─ Hocsinh
  │   └─ Dangky_group
  ├─ GCN_Nhansu
  ├─ Danhgia_taphuan -> KhoaTapHuan
  ├─ Giaovien_Taphuan -> KhoaTapHuan
  ├─ TeacherJoinClass -> Khoahoc
  └─ Lop_TKB
```

## Luồng dữ liệu chính

1. Admin tạo hồ sơ `Nhan_su`.
2. Một số flow tạo luôn `Users` và gán quyền theo vị trí.
3. Giáo viên được chọn trong `LOPHOC` qua `Lop_TKB.idgiaovien`.
4. Giáo viên có thể tham gia tập huấn trong `KHOADAOTAO`.
5. Event giáo viên lọc bằng `Nhan_su` và `Giaovien_Taphuan`.
6. Thống kê nhân sự dùng `Users.id` để đếm học viên/đăng ký/doanh thu.

## Issue DB cần chú ý

- `Nhan_su` và `Users` là quan hệ quan trọng nhưng chưa thấy constraint một-một rõ.
- `04NS ` có khoảng trắng dư trong permission check.
- `Giaovien_Taphuan` và `GiaoVien_Taphuan` có tên khác nhau, cần kiểm tra DB thực tế.
- `TeacherJoinClass` hard delete khi cancel.
- `Lop_TKB` bị update hàng loạt khi chuyển cơ sở/giáo viên.
- Nên kiểm tra index cho:
  - `Nhan_su(idchinhanh, enable, vitri, temp)`
  - `Users(idnhansu, enable, idChinhanh)`
  - `Lop_TKB(idgiaovien)`
  - `TeacherJoinClass(IdNhansu, IdKhoahoc, Fromdate, Todate)`
  - `GCN_Nhansu(idnhansu, ngaycap, ngayhethan)`
