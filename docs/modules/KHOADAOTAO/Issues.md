# Issues module khóa đào tạo

## Mức ưu tiên cao

### SQL injection do truy vấn SQL nối chuỗi trực tiếp

Nhiều file nối query string/form vào SQL: tạo khóa, sửa khóa, đăng ký, đánh giá, cập nhật thông tin nhân sự, import và thống kê. Cần sửa bằng parameterized query.

### Side effect tạo/kích hoạt tài khoản

Đánh giá tập huấn có thể tạo hoặc kích hoạt `Users`, cập nhật mật khẩu mặc định và hạn dùng. Đây là logic nhạy cảm cần transaction, audit và rule rõ ràng.

### Import dữ liệu đánh giá

Import có thể ghi nhiều bảng và reset trạng thái. Cần validate schema, chống trùng, log lỗi từng dòng và không ghi một phần khi batch lỗi.

## Mức ưu tiên trung bình

- Validate điểm số/ngày/phí/số lượng.
- Chuẩn hóa gửi email kết quả.
- Kiểm tra quyền giữa chi nhánh và HQ.
- Tối ưu thống kê theo chi nhánh/khóa/ngày.
- Encode output nhận xét, ghi chú, tên khóa.

## Cần test khi sửa

- Tạo khóa tập huấn.
- Đăng ký nhân sự.
- Cập nhật đánh giá.
- Import đánh giá.
- Gửi email kết quả.
- Báo cáo thống kê/kế toán.
