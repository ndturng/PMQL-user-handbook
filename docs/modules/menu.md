# Module menu (`MODULE/menu`)

## Mục đích

`MODULE/menu` chứa menu trái, menu top và script ribbon điều hướng toàn app.

## Trạng thái sử dụng trong app

Đây là module hạ tầng của flow chính. Menu quyết định user có nhìn thấy đường vào các module hay không.

## File chính

- `MODULE/menu/menu_left.ascx`
- `MODULE/menu/menu_top.ascx`
- `MODULE/menu/jsribbon.js`

## DB và tương tác

Menu thường dựa vào session user, level, chi nhánh và permission key. Các permission key đang rải trong nhiều module nên menu chỉ là một phần của kiểm soát quyền; code-behind từng module vẫn cần check lại.

## Vấn đề cần chú ý

- Cần đồng bộ permission key giữa menu và code xử lý. Có module menu hiện nhưng vào màn bị chặn hoặc ngược lại.
- Không nên xem ẩn menu là bảo mật; endpoint vẫn phải check quyền.
- Khi đổi route/module name, cần cập nhật menu và các link nội bộ.

## Ảnh hưởng sang module khác

Sai menu có thể làm user không vào được chức năng đang có quyền, hoặc nhìn thấy chức năng không nên thấy. Đây là điểm cần test sau khi sửa permission/module route.
