# HOCVIEN: Thống kê, báo cáo, import và in ấn

## Mục đích

Nhóm này bao gồm các màn không phải CRUD hồ sơ trực tiếp nhưng dùng dữ liệu học viên để báo cáo, import, thi TQ và in chứng chỉ/phiếu.

File tiêu biểu:

- `Thongke.ascx.cs`
- `Thongke_baoluu.ascx.cs`
- `Thongke_thitq.ascx.cs`
- `Danhsach_14days.ascx.cs`
- `Danhsach_nghihoc.ascx.cs`
- `Danhsach_daxoa.ascx.cs`
- `Danhsach_CNKK*.ascx.cs`
- `Danhsach_thitq.ascx.cs`
- `Nhapdiem_thitq*`
- `importList*.aspx.cs`
- `Print/`
- `Printcu/`

## Trạng thái sử dụng trong app

`home.ascx` link trực tiếp tới:

- `hocvien!thongke`
- `hocvien!thongke_baoluu`
- `hocvien!Danhsach_14days`
- `hocvien!Danhsach_CNKK`
- `hocvien!Danhsach_daxoa`
- Birthday picker/print

## Nhóm thống kê

| Feature | File | Permission |
| --- | --- | --- |
| Thống kê học viên | `Thongke.ascx.cs` | `01TK` |
| Thống kê bảo lưu | `Thongke_baoluu.ascx.cs` | `01TK` |
| Thống kê thi TQ | `Thongke_thitq.ascx.cs` | `01TK` |
| Gần kết khóa 14 ngày | `Danhsach_14days.ascx.cs` | `0114N` |
| Học viên nghỉ học/vắng | `Danhsach_nghihoc.ascx.cs` | `01HVV` |
| Học viên đã xóa | `Danhsach_daxoa.ascx.cs` | `01TK` |
| Cấp giấy chứng nhận | `Danhsach_CNKK*.ascx.cs` | `01GCN` / `01GCN ` |

## Import

Các file import:

- `importList.aspx.cs`
- `importList_new.aspx.cs`
- `importList_thitq.aspx.cs`

Nhóm này thường tương tác với Excel/file upload và insert/update hàng loạt. Khi sửa cần kiểm tra:

- Mapping cột import.
- Kiểm trùng học viên.
- Chi nhánh của user hiện tại.
- Encoding tiếng Việt.
- Rollback khi một dòng lỗi.

## Thi TQ

File liên quan:

- `Danhsach_thitq.ascx.cs`
- `Nhapdiem_thitq.ascx`
- `nhapdiem_thitq.aspx.cs`
- `nhapsbd_thitq.aspx.cs`
- `Thongke_thitq.ascx.cs`
- `Xephang_thitq.ascx.cs`

Permission quan sát được:

- `01TK` cho thống kê/list.
- `13NDI ` cho nhập điểm, có dấu cách trong permission key.

## In ấn

Folder:

- `MODULE/HOCVIEN/Print/`
- `MODULE/HOCVIEN/Printcu/`

Chứa:

- Template chứng chỉ.
- Phiếu đăng ký.
- Phiếu thu học phí HTML.
- Birthday print.
- Font/CSS/image assets.

`Printcu` có vẻ là bản cũ. Cần xác nhận bản nào đang được route gọi trước khi sửa template.

## DB tương tác

Tùy báo cáo, nhưng thường đọc:

- `Hocsinh`
- `Dangky`, `Dangky_group`
- `Lophoc_join`
- `Khoahoc`, `Chuongtrinh`
- `ChiNhanh`
- `Users`
- `Hocphi`, `ThuChi`
- Bảng thi TQ liên quan trong các file `thitq`

## Vấn đề cần chú ý

- Một số permission key có dấu cách cuối: `01GCN `, `13NDI `. Không tự sửa nếu chưa kiểm tra dữ liệu quyền.
- Nhiều báo cáo build HTML server-side và trả JSON/base64.
- **Date boundary không thống nhất giữa các báo cáo**.
- **Các report thường nối SQL trực tiếp từ filter**.
- Các folder `Print`/`Printcu` chứa nhiều asset lớn và duplicate; cần tránh sửa nhầm bản không còn dùng.
- Với import, không nên chạy trên live nếu chưa backup DB vì có thể tạo dữ liệu hàng loạt.
