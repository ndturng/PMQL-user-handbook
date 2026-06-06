# Module lỗi (`MODULE/erros`)

## Mục đích

`MODULE/erros` chứa các màn lỗi như `404` và `permistion`. Đây là module hạ tầng hiển thị khi user truy cập route không tồn tại hoặc không đủ quyền.

## Trạng thái sử dụng trong app

Đang được dùng gián tiếp trong flow chính vì nhiều module gọi kiểm tra quyền trước khi xử lý.

## File chính

- `MODULE/erros/404.ascx`
- `MODULE/erros/permistion.ascx`

## Vấn đề cần chú ý

- Tên folder/file có typo (`erros`, `permistion`), cần giữ nguyên nếu route hiện tại phụ thuộc vào tên này.
- Không nên để màn lỗi lộ query string, SQL hoặc thông tin server.
- Nên thống nhất redirect/hiển thị lỗi quyền giữa các module.

## Ảnh hưởng sang module khác

Mọi module có `Users.CheckPermisstion` hoặc check level đều có thể điều hướng tới màn lỗi quyền. Nếu sửa route lỗi, cần test menu chính và các màn thao tác admin.
