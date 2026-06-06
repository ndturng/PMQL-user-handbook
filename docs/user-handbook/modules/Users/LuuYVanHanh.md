# Lưu ý vận hành

## Quyền thao tác phụ thuộc tài khoản đăng nhập

Các nút như sửa, xóa, phân quyền, đổi trạng thái hoặc gia hạn chỉ hiển thị khi tài khoản đang đăng nhập có quyền phù hợp.

Nếu người dùng không thấy nút thao tác trên một dòng tài khoản, cần kiểm tra lại quyền của người dùng hiện tại trước khi kết luận lỗi dữ liệu.

## Không tự tạo tài khoản từ màn danh sách nếu không có nút hiển thị

Màn danh sách hiện tại không hiển thị nút thêm tài khoản cho luồng người dùng cuối. Vì vậy handbook không hướng dẫn thao tác tạo tài khoản mới từ màn này.

## Cẩn trọng khi đổi cấp bậc và phân quyền

Việc đổi `Cấp Bậc` hoặc phân quyền sai có thể làm người dùng thấy thiếu chức năng hoặc có quyền thao tác vượt phạm vi mong muốn.

Sau khi cập nhật quyền, nên đăng nhập kiểm tra bằng tài khoản vừa chỉnh nếu đây là tài khoản vận hành quan trọng.
