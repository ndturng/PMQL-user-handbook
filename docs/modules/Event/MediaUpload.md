# Upload ảnh event

## Feature

Feature này upload và xóa ảnh hiển thị cho sự kiện:

- `Icon`
- `DesktopBackground`
- `MobileBackground`

## Trạng thái trong flow chính

Có dùng trong flow quản lý sự kiện.

- Entry: `Default.aspx?mod=event!uploadImage`
- Code chính: `MODULE/Event/uploadImage.ascx.cs`
- UI liên quan: `MODULE/Event/uploadImage.ascx`
- Trigger client: `MODULE/Event/js/event.js`

## Command liên quan

| Command | Vai trò |
| --- | --- |
| `cmd=load` | Đọc ảnh hiện tại từ bảng `Event`. |
| `cmd=save` | Lưu file vào `/uploads`, update cột ảnh trong `Event`. |
| `cmd=delete` | Set cột ảnh về `NULL` và xóa file vật lý nếu tìm được. |

## DB/filesystem tương tác

| Thành phần | Vai trò |
| --- | --- |
| `Event.Icon` | Ảnh icon event. |
| `Event.DesktopBackground` | Ảnh background desktop. |
| `Event.MobileBackground` | Ảnh background mobile. |
| `/uploads` | Folder lưu file upload. |

## Tương tác với module khác

- `List.ascx`/JS mở UI upload ảnh từ màn quản lý event.
- Ảnh được lưu trong bảng `Event`, nên module legacy `Sukien` cũng có thể thấy hoặc làm lệch dữ liệu nếu dùng cùng bảng.
- Vì workspace hiện tại chỉ là local editing, thay đổi code upload không tự triển khai lên server remote; user tự upload qua FTP.

## Vấn đề cần sửa

- Cần validate upload file:
  - extension whitelist
  - MIME type
  - size tối đa
  - tên file/path
  - quyền user
- Cần tránh xóa file vật lý nếu file đang được event khác dùng chung.
- Cần chuẩn hóa cách lưu path/URL để khớp môi trường remote server.
- SQL update ảnh còn cần kiểm tra parameterization.
