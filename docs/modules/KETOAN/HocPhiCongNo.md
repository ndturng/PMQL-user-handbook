# Học phí và công nợ

## Feature

Feature này mô tả phần học phí/công nợ liên quan kế toán. Cần lưu ý: flow thu học phí thật nằm trong `MODULE/HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs`, không nằm trực tiếp trong `MODULE/KETOAN`.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Từ lịch sử đăng ký học viên mở:

```text
frame.aspx?mod=ketoan!phieuthu_hocphi&idlenh={idDangkyGroup}
```

- `MODULE/KETOAN/hocphi_log` là màn log học phí cũ.
- `MODULE/KETOAN/baocao_congno` là màn xem công nợ.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs` | Flow thu học phí chính, ghi `ThuChi` và `Congno`. |
| `MODULE/KETOAN/Hocphi_log.ascx.cs` | Log phiếu thu học phí cũ từ bảng `Hocphi`. |
| `MODULE/KETOAN/Baocao_congno.ascx.cs` | Báo cáo công nợ theo thời gian/chi nhánh/type. |
| `MODULE/KETOAN/View/View_congno.ascx.cs` | Popup chi tiết công nợ còn phải thu/trả. |
| `MODULE/HOCVIEN/DANGKY/Lichsu_dangky.ascx` | Mở popup thanh toán học phí. |
| `MODULE/HOCVIEN/History-registry.ascx` | Mở popup thanh toán học phí. |
| `MODULE/HOCVIEN/Print/*` | In phiếu/summary dựa trên `ThuChi` hoặc `Hocphi` tùy template. |

## Logic thu học phí

`Phieuthu_hocphi.aspx.cs`:

1. Check permission `01THP`.
2. Load phiếu đăng ký từ `Dangky_group` và học viên từ `Hocsinh`.
3. Tính số tiền còn phải thanh toán:
   - tổng tiền phiếu đăng ký
   - trừ các phiếu thu `ThuChi.type='1'`
   - trừ chiết khấu `ThuChi.type='0'`
   - trừ các giảm giá/vật tư/thu khác liên quan.
4. Khi tạo mới:
   - validate số tiền thu không vượt tổng còn lại.
   - nếu có chiết khấu, insert một dòng `ThuChi` type `0`, `loai='dangkykhoahoc'`.
   - nếu còn nợ, insert/update `Congno`.
   - insert phiếu thu `ThuChi` type `1`, `loai='dangkykhoahoc'`.
   - update `Dangky_group.status`, `Dangky.status`, `Dangky_thukhac.status`, `KHO_Chitietphieu.status`.
   - cập nhật tồn kho nếu phiếu có vật tư.
   - cập nhật voucher nếu có dùng voucher.

## Logic công nợ

`Congno` được tạo/cập nhật trong flow thu học phí:

- Nếu còn nợ sau khi thu tiền, tạo `Congno` type `1`, `loai='dangkykhoahoc'`, `ngayhen = GETDATE()+14`.
- Nếu đã có công nợ cho phiếu đăng ký, update `thanhtoan += tienthu`.

`Baocao_congno.ascx.cs` load `Congno` theo:

- `type`
- `idchinhanh`
- `updatetime` trong khoảng ngày

`View_congno.ascx.cs` hiển thị phần còn nợ:

```text
tongtien - thanhtoan > 0
```

## Log học phí cũ

`Hocphi_log.ascx.cs`:

- Check permission `05PTHP`.
- `cmd=logphieuthu`: query bảng `Hocphi`, join `Hocsinh`, `Dangky_group`, `Users`.
- `cmd=Del_phieuthuhocphi`: nếu có action xóa `05PTHP`, delete thẳng row trong `Hocphi`.

Trong UI có ghi chú “Phiếu thu học phí (Dữ liệu cũ)”, nên cần coi bảng `Hocphi` là legacy hoặc dữ liệu cũ, không phải source chính hiện tại nếu flow mới dùng `ThuChi`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `ThuChi` | Phiếu thu học phí, phiếu chi chiết khấu, dữ liệu báo cáo. |
| `Congno` | Công nợ cần thu/cần chi. |
| `Dangky_group` | Phiếu đăng ký khóa học tổng. |
| `Dangky` | Dòng khóa học trong phiếu đăng ký. |
| `Dangky_thukhac` | Thu khác/voucher/chiết khấu liên quan đăng ký. |
| `KHO_Chitietphieu`, `KHO` | Vật tư đi kèm đăng ký và tồn kho. |
| `Hocsinh` | Học viên thanh toán. |
| `Hocsinh_voucher` | Voucher dùng khi thanh toán. |
| `Hocphi` | Bảng học phí cũ/log cũ. |

## Tương tác với module khác

- `HOCVIEN` là nơi khởi tạo phiếu đăng ký và mở thanh toán học phí.
- `KHO` bị cập nhật tồn kho khi thanh toán phiếu có vật tư.
- `THITQ` có logic update khi có thu khác trong phiếu đăng ký.
- Báo cáo `KETOAN` đọc `ThuChi` và `Congno` do flow học phí tạo.

## Overlap/xung đột

- `Hocphi` và `ThuChi` cùng liên quan học phí, nhưng `Hocphi` có vẻ là dữ liệu cũ.
- Print template cũ có thể còn đọc `Hocphi`, trong khi flow mới đọc `ThuChi`.
- Công nợ được update bằng cộng dồn `thanhtoan`, nhưng không thấy transaction bao toàn bộ flow thu học phí.

## Vấn đề cần sửa

- Cần xác định rõ source of truth học phí hiện tại: `ThuChi` hay `Hocphi`.
- Flow thu học phí cần transaction vì đang insert/update nhiều bảng: `ThuChi`, `Congno`, `Dangky_group`, `Dangky`, `KHO`, voucher.
- Mã phiếu học phí cũng sinh bằng `count()+1`, có nguy cơ trùng.
- `Del_phieuthuhocphi` delete thẳng bảng `Hocphi`, có thể làm mất lịch sử.
- SQL injection do truy vấn SQL nối chuỗi trực tiếp từ request/form.
