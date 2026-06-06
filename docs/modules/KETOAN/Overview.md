# Module Kế toán (`MODULE/KETOAN`)

## Mục đích

`MODULE/KETOAN` là module theo dõi thu/chi và báo cáo tài chính vận hành. Module này không chứa toàn bộ logic phát sinh tiền của hệ thống; nhiều phiếu thu/chi được tạo từ module khác rồi cùng ghi vào bảng `ThuChi`.

Các nhóm chính:

- Tạo phiếu thu/phiếu chi thủ công.
- Xem nhật ký thu chi.
- Xem báo cáo tháng/năm, doanh thu, lợi nhuận.
- Xem công nợ.
- Xem/xóa log phiếu thu học phí cũ.

Entry chính:

```text
Default.aspx?mod=ketoan!home
```

## Trạng thái sử dụng trong app

Module này đang nằm trong flow chính của app.

Bằng chứng:

- `MODULE/menu/menu_left.ascx` trỏ menu kế toán tới `Default.aspx?mod=ketoan!home`.
- `MODULE/KETOAN/home.ascx` có các entry tạo phiếu thu/chi, nhật ký, báo cáo tháng/năm, học phí.
- `MODULE/HOCVIEN` mở popup thu học phí qua route kế toán/học phí.
- `MODULE/Event/List.ascx.cs` tạo phiếu thu/chi event vào bảng `ThuChi`.

Kết luận thực tế: `KETOAN` là **điểm tổng hợp dữ liệu tiền**, nhưng **source phát sinh tiền nằm rải rác** ở `HOCVIEN`, `Event`, kho/vật tư và các form thủ công.

## Tài liệu con

- [ThuChi.md](./ThuChi.md): phiếu thu/chi thủ công và nhật ký thu chi.
- [HocPhiCongNo.md](./HocPhiCongNo.md): học phí, công nợ và flow liên quan `HOCVIEN`.
- [BaoCao.md](./BaoCao.md): báo cáo doanh thu/lợi nhuận/tháng/năm và view chi tiết.
- [DataModel.md](./DataModel.md): bảng dữ liệu và quan hệ.
- [Issues.md](./Issues.md): rủi ro logic, DB, bảo mật, hiệu suất và việc cần sửa.

## File liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/KETOAN/home.ascx` | Dashboard kế toán. |
| `MODULE/KETOAN/Menu_top.ascx` | Menu con kế toán. |
| `MODULE/KETOAN/js/ketoan.js` | JS mở popup phiếu thu/chi, load nhật ký/công nợ, export Excel. |
| `MODULE/KETOAN/Phieuthu_Auto.ascx.cs` | Tạo phiếu thu thủ công vào `ThuChi`. |
| `MODULE/KETOAN/PhieuChi_Auto.ascx.cs` | Tạo phiếu chi thủ công vào `ThuChi`, có kiểm tra tiền tồn. |
| `MODULE/KETOAN/Nhatky_thuchi.ascx.cs` | Nhật ký thu chi, filter và thao tác xóa đặc biệt. |
| `MODULE/KETOAN/Hocphi_log.ascx.cs` | Log phiếu thu học phí cũ từ bảng `Hocphi`. |
| `MODULE/KETOAN/Baocao_congno.ascx.cs` | Báo cáo công nợ từ bảng `Congno`. |
| `MODULE/KETOAN/Baocao_loinhuanthang.ascx.cs` | Báo cáo tháng từ `ThuChi` và đăng ký khóa học. |
| `MODULE/KETOAN/Baocao_loinhuannam.ascx.cs` | Báo cáo năm từ `ThuChi`. |
| `MODULE/KETOAN/LOINHUAN.ascx.cs` | Báo cáo lợi nhuận theo khoảng ngày, có công nợ. |
| `MODULE/KETOAN/View/*` | Popup view chi tiết doanh thu, phiếu thu, công nợ, thu/chi ngày. |
| `MODULE/HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs` | Flow thu học phí thật, ghi `ThuChi` và `Congno`. |
| `MODULE/Event/List.ascx.cs` | Flow thu/hoàn phí event, ghi `ThuChi` loại `eventsukien`. |

## Feature map

| Feature | Entry/command | File chính | Bảng chính |
| --- | --- | --- | --- |
| Phiếu thu thủ công | `Frame-ajax.aspx?mod=ketoan!phieuthu_auto`, `cmd=save` | `Phieuthu_Auto.ascx.cs` | `ThuChi` |
| Phiếu chi thủ công | `Frame-ajax.aspx?mod=ketoan!phieuchi_auto`, `cmd=save` | `PhieuChi_Auto.ascx.cs` | `ThuChi` |
| Nhật ký thu chi | `ketoan!nhatky_thuchi`, `cmd=load` | `Nhatky_thuchi.ascx.cs` | `ThuChi` |
| Xóa phiếu nhật ký | `cmd=remove` | `Nhatky_thuchi.ascx.cs` | `ThuChi` |
| Log học phí cũ | `ketoan!hocphi_log`, `cmd=logphieuthu` | `Hocphi_log.ascx.cs` | `Hocphi` |
| Xóa log học phí cũ | `cmd=Del_phieuthuhocphi` | `Hocphi_log.ascx.cs` | `Hocphi` |
| Báo cáo công nợ | `ketoan!baocao_congno`, `cmd=load` | `Baocao_congno.ascx.cs` | `Congno` |
| Báo cáo tháng | `ketoan!baocao_loinhuanthang`, `cmd=load` | `Baocao_loinhuanthang.ascx.cs` | `ThuChi`, `Dangky`, `Dangky_group` |
| Báo cáo năm | `ketoan!baocao_loinhuannam`, `cmd=load` | `Baocao_loinhuannam.ascx.cs` | `ThuChi` |
| Báo cáo lợi nhuận | `ketoan!LOINHUAN`, `cmd=load` | `LOINHUAN.ascx.cs` | `ThuChi`, `Congno` |
| Thu học phí | `frame.aspx?mod=ketoan!phieuthu_hocphi` | `MODULE/HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs` | `ThuChi`, `Congno`, `Dangky_group` |
| Thu/hoàn phí event | `cmd=create_payment`, `cmd=refund_payment` | `MODULE/Event/List.ascx.cs` | `ThuChi`, `EventPaymentConfirmation` |

## Permission key chính

| Permission | Ý nghĩa quan sát được |
| --- | --- |
| `05PTC` | Tạo phiếu thu/chi thủ công. |
| `05TK` | Xem nhật ký và báo cáo thống kê. |
| `05CN` | Xem công nợ. |
| `05PTHP` | Xem/xóa log phiếu thu học phí cũ. |
| `01THP` | Thu học phí trong flow `HOCVIEN/DANGKY/Phieuthu_hocphi.aspx.cs`. |

## Nhận xét nhanh

`ThuChi` là bảng trung tâm và được dùng bởi nhiều module. Khi sửa `KETOAN`, cần kiểm tra tác động tới `HOCVIEN`, `Event`, kho/vật tư và báo cáo. Rủi ro lớn nhất hiện tại là sinh mã phiếu bằng `count()+1`, SQL injection do truy vấn SQL nối chuỗi trực tiếp, nhiều flow không dùng transaction đầy đủ và logic “xóa” phiếu không thống nhất giữa `ThuChi` và `Hocphi`.
