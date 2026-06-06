# Data model module Kế toán

## Bảng trung tâm

### `ThuChi`

`ThuChi` là bảng trung tâm cho phiếu thu/chi và phần lớn báo cáo tài chính.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính. |
| `code` | Mã phiếu thu/chi, ví dụ `PT...`, `PC...`. |
| `type` | `1` là thu, `0` là chi. |
| `idChinhanh` | Chi nhánh ghi nhận phiếu. |
| `iduser` | User tạo phiếu. |
| `idhocvien` | Học viên liên quan; trong event có thể chứa participant id cả giáo viên. |
| `nguoinhan` | Người nộp/người nhận khi không gắn học viên. |
| `dienthoai`, `diachi` | Thông tin người nộp/nhận. |
| `tongtien` | Số tiền. |
| `bangchu` | Số tiền bằng chữ. |
| `lydo` | Lý do thu/chi. |
| `loai` | Loại nghiệp vụ: `khac`, `dangkykhoahoc`, `eventsukien`, `vattu`, `donhang`, `phithuonghieu`, `phidaotao`, ... |
| `updatetime` | Ngày ghi nhận. |
| `hinhthuc` | Hình thức thanh toán: `tienmat`, `chuyenkhoan`, `quetthe`, khác. |
| `sochungtu` | Số chứng từ ngoài. |
| `idphieudangky` | Phiếu đăng ký khóa học liên quan. |

## Bảng công nợ

### `Congno`

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính. |
| `type` | `1` là nợ cần thu, `0` là nợ cần chi. |
| `idChinhanh` | Chi nhánh. |
| `iduser` | User tạo/cập nhật. |
| `loai` | Loại nghiệp vụ, ví dụ `dangkykhoahoc`. |
| `idncc` | Nhà cung cấp nếu là nợ cần chi. |
| `idhocvien` | Học viên nếu là nợ học phí. |
| `tongtien` | Tổng tiền nợ. |
| `thanhtoan` | Số đã thanh toán. |
| `status` | Trạng thái. |
| `idphieudangky` | Phiếu đăng ký liên quan. |
| `ngayhen` | Ngày hẹn thanh toán. |
| `lydo` | Lý do công nợ. |
| `updatetime` | Ngày tạo/cập nhật. |

## Bảng học phí cũ

### `Hocphi`

Được `Hocphi_log.ascx.cs` đọc/xóa. UI ghi là “Phiếu thu học phí (Dữ liệu cũ)”.

| Cột | Ý nghĩa quan sát được |
| --- | --- |
| `id` | Khóa chính. |
| `MAHP`/`mahp` | Mã phiếu học phí. |
| `iddangkygroup` | Phiếu đăng ký group. |
| `iduser` | Người thu. |
| `ngaythu` | Ngày thu. |
| `thanhtoan` | Số thanh toán. |
| `thucthu` | Số thực thu. |
| `ghichu` | Ghi chú. |

## Bảng ngoài module nhưng ảnh hưởng kế toán

| Bảng | Vai trò |
| --- | --- |
| `Dangky_group` | Phiếu đăng ký khóa học tổng, có `tongtien`, `status`, `madk`. |
| `Dangky` | Dòng khóa học, có `Hocphi`, `Giam`, `Tonghocphi`, `status`. |
| `Dangky_thukhac` | Thu khác, voucher, chiết khấu theo phiếu đăng ký. |
| `Hocsinh` | Học viên liên quan phiếu thu/công nợ. |
| `Hocsinh_voucher` | Voucher dùng khi thanh toán. |
| `KHO_Chitietphieu`, `KHO` | Vật tư theo phiếu đăng ký và tồn kho. |
| `EventPaymentConfirmation` | Trạng thái thu/hoàn phí event, liên kết với `ThuChi`. |
| `Users` | Người tạo phiếu. |
| `ChiNhanh` | Phân quyền và filter dữ liệu theo chi nhánh. |

## Quan hệ dữ liệu chính

```text
ThuChi
  ├─ idhocvien -> Hocsinh
  ├─ idphieudangky -> Dangky_group
  └─ iduser -> Users

Dangky_group
  ├─ Dangky
  ├─ Dangky_thukhac
  ├─ Congno
  ├─ ThuChi
  └─ KHO_Chitietphieu -> KHO

EventPaymentConfirmation
  └─ ReceiptCode/ReceiptSource -> ThuChi
```

## Luồng dữ liệu chính

1. Phiếu thu/chi thủ công ghi trực tiếp `ThuChi`.
2. Thu học phí trong `HOCVIEN` ghi `ThuChi`, có thể ghi/cập nhật `Congno`, cập nhật trạng thái đăng ký và tồn kho.
3. Thu/hoàn phí event ghi `EventPaymentConfirmation` và `ThuChi`.
4. Báo cáo kế toán đọc `ThuChi`, `Congno`, đôi khi đọc thêm `Dangky`/`Dangky_group`.
5. Log học phí cũ đọc bảng `Hocphi`.

## Issue DB cần chú ý

- `ThuChi.code` được sinh bằng count ở nhiều flow, dễ trùng nếu concurrent.
- `type` dùng string `'0'`/`'1'`; nên chuẩn hóa enum hoặc constraint.
- `loai` là text nghiệp vụ, không thấy bảng lookup/constraint.
- `ThuChi.idhocvien` bị dùng cho nhiều ngữ cảnh, trong event giáo viên có thể không thật sự là học viên.
- `Hocphi` và `ThuChi` cùng liên quan học phí, cần xác định rõ dữ liệu nào là chính.
- Nên kiểm tra index cho:
  - `ThuChi(idChinhanh, updatetime, type, loai)`
  - `ThuChi(idphieudangky, type)`
  - `Congno(idChinhanh, type, updatetime)`
  - `Congno(idphieudangky)`
  - `Hocphi(iddangkygroup, ngaythu)`
