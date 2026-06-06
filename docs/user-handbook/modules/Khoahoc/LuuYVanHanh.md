# Lưu ý vận hành module Khóa học

## Nguyên tắc chung

Module Khóa học là dữ liệu nền. Sửa sai chương trình, khóa học, học phí hoặc cấu hình cơ sở có thể ảnh hưởng trực tiếp đến đăng ký học viên, lớp học, học phí, sự kiện và báo cáo.


## Các điểm dễ sai

| Tình huống | Rủi ro |
| --- | --- |
| Sửa học phí khóa học đã có đăng ký | Có thể làm lệch thu học phí hoặc công nợ. |
| Đổi cấp độ hàng loạt | Có thể đổi nhầm nhiều khóa cùng lúc. |
| Xóa khóa học đã có đăng ký | Có thể làm mất đường đối chiếu dữ liệu. |
| Phân bổ sai cơ sở | Cơ sở có thể đăng ký nhầm khóa hoặc không thấy khóa cần dùng. |
| Cấu hình học phí cơ sở thấp hơn học phí cơ bản | Hệ thống có thể chặn hoặc tự đưa về giá tối thiểu. |
| Gắn sai vật tư/tài liệu | Ảnh hưởng chuẩn bị học liệu và kho. |

## Trình tự vận hành khuyến nghị

1. Tạo hoặc cập nhật chương trình.
2. Tạo khóa học và kiểm tra cấp độ, mã khóa, học phí, số giờ.
3. Cài đặt trạng thái/hiển thị nếu cần.
4. Gắn vật tư hoặc tài liệu đi kèm.
5. Phân bổ khóa học sang cơ sở.
6. Kiểm tra học phí, số giờ, phí online và phí test theo cơ sở.
7. Cho phép cơ sở dùng khóa trong đăng ký học viên.
8. Theo dõi số liệu ở thống kê đăng ký.

## Khi cần phối hợp với bộ phận khác

Cần phối hợp với kế toán khi:

- thay đổi học phí
- thay đổi phí online hoặc phí test
- khóa đã có học viên đăng ký hoặc công nợ
- số liệu thống kê lệch với học phí đã thu

Cần phối hợp với vận hành/lớp học khi:

- đổi số giờ/số buổi
- đổi cấp độ
- khóa đang dùng để xếp lớp
- khóa liên quan đến điểm danh/kết quả

Cần phối hợp với kho khi:

- thêm/xóa vật tư hoặc tài liệu
- thay đổi số lượng vật tư đi kèm
- khóa đang phát sinh chuẩn bị học liệu

## Checklist trước khi sửa dữ liệu nền

- Đã kiểm tra khóa có đăng ký học viên chưa.
- Đã kiểm tra khóa có đang được phân bổ về cơ sở nào.
- Đã kiểm tra khóa có đang dùng trong lớp học hoặc sự kiện không.
- Đã thống nhất với kế toán nếu sửa học phí/phí online/phí test.
- Đã thống nhất với vận hành nếu sửa số giờ, cấp độ hoặc vật tư.
- Đã lưu lại lý do điều chỉnh nếu nghiệp vụ yêu cầu.
