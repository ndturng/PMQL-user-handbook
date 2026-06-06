# HOCVIEN: Data model

## Bảng trung tâm

### `Hocsinh`

Nguồn hồ sơ học viên.

Các cột xuất hiện nhiều trong code:

| Cột | Ý nghĩa |
| --- | --- |
| `id` | Khóa chính học viên |
| `MaHS` / `mahs` | Mã học viên |
| `ten` | Tên học viên |
| `email` | Email |
| `dienthoai`, `PHHS_dienthoai` | Số điện thoại học viên/phụ huynh |
| `PHHS_diachi` | Địa chỉ |
| `namsinh` | Năm sinh, dùng tính tuổi |
| `idChiNhanh` | Chi nhánh |
| `iduser` | User quản lý/tạo |
| `enable` | Xóa mềm |
| `phhs_mkt` | Chương trình marketing |
| `status_dkcu` | Cho phép đăng ký khóa cũ |
| `Lophoc` | Mapping lớp tuổi/cấp học trong form auto |

## Đăng ký khóa học

### `Dangky_group`

Phiếu đăng ký tổng, gắn với học viên.

| Cột | Ý nghĩa |
| --- | --- |
| `id` | Khóa phiếu |
| `MADK` / `madk` | Mã phiếu |
| `IDhocsinh` | Học viên |
| `IDuser` | User lập phiếu |
| `idchinhanh` | Chi nhánh |
| `tongtien` | Tổng tiền phiếu |
| `updatetime` | Ngày cập nhật |
| `status` | Trạng thái phiếu |
| `ghichu` | Ghi chú |
| `enable` | Còn hiệu lực |

### `Dangky`

Dòng đăng ký từng khóa học.

| Cột | Ý nghĩa |
| --- | --- |
| `id` | Khóa dòng |
| `iddangkygroup` | Phiếu tổng |
| `IDhocsinh` | Học viên |
| `IDkhoahoc` | Khóa học |
| `Hocphi` | Học phí gốc |
| `Giam` | Giảm giá |
| `Tonghocphi` | Phải thu |
| `khuyenmaiid` | Khuyến mãi |
| `enable` | Còn hiệu lực |
| `status_baoluu` | Trạng thái bảo lưu |
| `date_baoluu` | Ngày bảo lưu |
| `daysleft_baoluu` | Số ngày bảo lưu |
| `status_giahan` | Gia hạn |

### `Lophoc_join`

Quan hệ học viên/lớp/đăng ký.

| Cột | Ý nghĩa |
| --- | --- |
| `idhocvien` | Học viên |
| `idkhoahoc` | Khóa học |
| `iddangky` | Dòng đăng ký |
| `idlop` | Lớp học |
| `fromdate`, `todate` | Thời gian học |
| `fromdate_temp`, `todate_temp` | Ngày tạm khi bảo lưu/kết thúc sớm |
| `status_end` | Kết thúc sớm |

## Bảo lưu/voucher/gia hạn

| Bảng | Vai trò |
| --- | --- |
| `Baoluu_khoahoc` | Khoảng bảo lưu khóa học theo `iddangky` |
| `Hocsinh_voucher` | Voucher/bảo lưu học phí |
| `Giahan_khoahoc` | Lịch sử gia hạn khóa học |

## Tài chính

| Bảng | Vai trò |
| --- | --- |
| `Hocphi` | Bảng học phí legacy |
| `ThuChi` | Phiếu thu/chi mới |
| `Dangky_thukhac` | Khoản thu khác trong phiếu đăng ký |
| `Hocsinh_voucher` | Voucher được dùng/ghi nhận bảo lưu |

## Kho/vật tư

| Bảng | Vai trò |
| --- | --- |
| `Khoahoc_kho` | Cấu hình vật tư đi kèm khóa học |
| `donhang_setting` | Thông tin vật tư/đơn giá |
| `KHO_Chitietphieu` | Chi tiết nhập/xuất vật tư phát sinh từ đăng ký |
| `Taisan_Nhap_Xuat` | Bảng cũ/khác liên quan vật tư đăng ký |

## Bảng hỗ trợ

| Bảng | Vai trò |
| --- | --- |
| `Users` | User tạo/quản lý |
| `ChiNhanh` | Chi nhánh/cơ sở |
| `Config`, `Config_data_text` | Cấu hình hệ thống/marketing |
| `Nhansu_Log` | Log thao tác học viên |
| `Khoahoc`, `Chuongtrinh`, `Lophoc` | Danh mục đào tạo/lớp |

## Bảng Event bị tác động

| Bảng | Vai trò |
| --- | --- |
| `Event` | Event học viên có thể đăng ký |
| `EventObject`, `EventTarget`, `EventDisplay` | Rule event phù hợp |
| `EventRegistration` | Đăng ký event của học viên |

## Lưu ý source of truth

- Trạng thái học viên **không nằm ở một bảng duy nhất**.
- "Đang học" phụ thuộc `Dangky.enable`, `Lophoc_join.todate`, `status_end`.
- "Bảo lưu" phụ thuộc `Baoluu_khoahoc` và `Dangky.status_baoluu`.
- Học phí đã thu có thể nằm ở `Hocphi` hoặc `ThuChi`.
- Xóa học viên chỉ set `Hocsinh.enable=0`, không tự hủy các quan hệ khác.
