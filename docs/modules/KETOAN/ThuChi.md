# Phiếu thu/chi và nhật ký thu chi

## Feature

Feature này xử lý tạo phiếu thu/chi thủ công và xem nhật ký thu chi từ bảng `ThuChi`.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Dashboard: `Default.aspx?mod=ketoan!home`
- Popup phiếu thu: `Frame-ajax.aspx?mod=ketoan!phieuthu_auto`
- Popup phiếu chi: `Frame-ajax.aspx?mod=ketoan!phieuchi_auto`
- Nhật ký: `Default.aspx?mod=ketoan!nhatky_thuchi`

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/KETOAN/Phieuthu_Auto.ascx.cs` | Insert phiếu thu thủ công vào `ThuChi`. |
| `MODULE/KETOAN/PhieuChi_Auto.ascx.cs` | Insert phiếu chi thủ công vào `ThuChi`. |
| `MODULE/KETOAN/Nhatky_thuchi.ascx.cs` | Load/filter nhật ký thu chi và thao tác remove. |
| `MODULE/KETOAN/js/ketoan.js` | Mở popup phiếu thu/chi, load nhật ký. |
| `MODULE/KETOAN/home.ascx` | Entry dashboard. |
| `MODULE/KETOAN/Menu_top.ascx` | Entry menu con. |

## Logic tạo phiếu thu thủ công

`Phieuthu_Auto.ascx.cs`:

1. Check permission `05PTC`.
2. Nhận dữ liệu từ query string: họ tên, điện thoại, địa chỉ, số tiền, bằng chữ, lý do, hình thức, số chứng từ.
3. Sinh mã phiếu:

```text
PT + năm + idChiNhanh + "00" + count(ThuChi theo chi nhánh) + 1
```

4. Insert vào `ThuChi`:
   - `type = '1'`
   - `loai = 'khac'`
   - `updatetime = getdate()`

## Logic tạo phiếu chi thủ công

`PhieuChi_Auto.ascx.cs`:

1. Check permission `05PTC`.
2. Tính tiền tồn bằng cách:
   - tổng thu trước hôm nay
   - trừ tổng chi trước hôm nay
   - cộng thu trong ngày
   - trừ chi trong ngày
3. Nếu tiền còn lại sau khi chi `<= 0` thì báo không đủ tiền tồn.
4. Sinh mã phiếu:

```text
PC + năm + idChiNhanh + "00" + count(ThuChi theo chi nhánh) + 1
```

5. Insert vào `ThuChi`:
   - `type = '0'`
   - `loai = 'khac'`

## Logic nhật ký thu chi

`Nhatky_thuchi.ascx.cs`:

- Check permission `05TK`.
- `cmd=load`: filter `ThuChi` theo:
  - `type`
  - `loai`
  - `hinhthuc`
  - `tungay`, `denngay`
  - `chinhanh`
- Render HTML rows rồi trả JSON base64.

`cmd=remove`:

- Chỉ hiện nút xóa nếu `checkusers.iduser()=="3384"`.
- Không xóa row, mà update:

```sql
Update ThuChi set tongtien='0' where id='{id}'
```

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `ThuChi` | Bảng chính của phiếu thu/chi và nhật ký. |
| `Hocsinh` | Hiển thị tên học viên khi `ThuChi.idhocvien` có giá trị. |
| `ChiNhanh` | Filter dữ liệu theo chi nhánh user được phép xem. |

## Tương tác với module khác

- `HOCVIEN` tạo phiếu thu học phí vào `ThuChi` với `loai='dangkykhoahoc'`.
- `Event` tạo phiếu thu/chi event vào `ThuChi` với `loai='eventsukien'`.
- Kho/vật tư có thể ghi `ThuChi` với các loại liên quan vật tư/đơn hàng.
- Báo cáo trong `KETOAN` đọc lại tất cả dòng `ThuChi`.

## Overlap/xung đột

- Phiếu thu/chi thủ công sinh `loai='khac'`; các flow nghiệp vụ khác sinh `loai` riêng. Báo cáo phụ thuộc vào `loai`, nên sửa tên loại sẽ ảnh hưởng thống kê.
- Sinh mã phiếu bằng `count()+1` có nguy cơ trùng khi nhiều user tạo cùng lúc.
- Event đã có hàm sinh mã có lock riêng, nhưng phiếu thủ công và học phí không dùng cùng cơ chế.

## Vấn đề cần sửa

- Sửa lỗi SQL injection: thay truy vấn SQL nối chuỗi trực tiếp từ request bằng parameterized query.
- Sinh mã phiếu cần transaction/lock hoặc sequence riêng.
- `remove` đưa `tongtien=0` làm mất số liệu gốc và không có audit rõ ràng.
- Cần thống nhất cách hủy/xóa phiếu giữa `ThuChi`, `Hocphi`, `Congno`, `EventPaymentConfirmation`.
- Nên thêm permission/action rõ cho thao tác remove, thay vì hard-code user id `3384`.
