# HOCVIEN: Vấn đề cần sửa và rủi ro

## Ưu tiên cao

- Sửa lỗi SQL injection bằng parameterized query cho các flow nhận input từ query/form: danh sách, thêm/sửa hồ sơ, đăng ký khóa, bảo lưu, thống kê.
- Bọc transaction cho các thao tác nhiều bảng: xóa phiếu đăng ký, hủy khóa học, bảo lưu/kích hoạt lại, gia hạn, insert đăng ký có vật tư.
- Đồng bộ flow đăng ký event từ HOCVIEN với flow chính trong `MODULE/Event`; ít nhất chặn event có phí nếu vẫn dùng `event!manage`.
- Xác định source of truth học phí: `Hocphi` hay `ThuChi`; hiện có màn phải fallback giữa hai bảng.
- Không xóa cứng dòng nghiệp vụ tài chính/đăng ký nếu đã phát sinh phiếu thu, lớp học, vật tư.
- Kiểm tra lại `Cancel_khoahoc()` vì đang tạo voucher bằng `Tonghocphi` khi hủy khóa, có thể sai nếu học viên đã học/đã thu một phần.

## Ưu tiên trung bình

- Phân trang danh sách học viên ở SQL thay vì load toàn bộ dataset rồi cắt trong memory.
- Chuẩn hóa permission key có dấu cách cuối: `01GCN `, `13NDI `, `01TDV `.
- Chuẩn hóa rule date boundary cho thống kê, bảo lưu, event, lớp học.
- Gom logic trạng thái học viên thành helper/service dùng chung thay vì lặp SQL `case` ở từng màn.
- Xem lại các file duplicate/cũ: `Printcu`, `hocvien_history1932025`, `Danhsach_14days1932025`, `dangky_togroup-copy`.
- Validate upload/import file: extension, size, content, rollback khi lỗi.

## Ưu tiên thấp

- Chuẩn hóa response JSON: nhiều nơi nối chuỗi thủ công, nơi khác encode base64/jsonBase.
- Chuẩn hóa tiếng Việt có dấu trong message.
- Tách bớt logic khỏi code-behind lớn.
- Tài liệu hóa metadata `Form_auto` vì nó quyết định form `Hocsinh`.

## Rủi ro logic theo feature

### Danh sách học viên

- Trạng thái học viên là derived state từ nhiều bảng, dễ lệch nếu update thủ công.
- Xóa học viên không hủy đăng ký/học phí/lớp liên quan.

### Hồ sơ học viên

- Form động phụ thuộc metadata; sửa field có thể ảnh hưởng cả thêm/sửa/filter.
- Mã học viên tạo sau insert, cần kiểm tra trùng/race.

### Đăng ký khóa học

- Xóa phiếu đăng ký group hiện xóa nhiều bảng không transaction.
- Vật tư kho insert theo đăng ký nhưng kiểm tồn kho có đoạn đang comment.
- Học phí đọc từ cả bảng cũ và mới.

### Bảo lưu

- Bảo lưu học phí và bảo lưu khóa học là hai nghiệp vụ khác nhau nhưng tên file/flow dễ gây nhầm.
- Kích hoạt lại bảo lưu tính ngày còn lại rồi update `Lophoc_join.todate`; sai công thức sẽ ảnh hưởng lịch học.

### Event registration

- Flow từ HOCVIEN không xử lý payment/ConsumedAt như flow Event chính.

## Kiểm tra trước khi sửa

1. Xác định entry URL thực tế đang gọi file nào.
2. Kiểm tra permission key tương ứng.
3. Kiểm tra bảng nào bị ghi, bảng nào chỉ đọc.
4. Với thao tác hủy/xóa, kiểm tra học phí, lớp, vật tư, log.
5. Với thống kê, kiểm tra date boundary và chi nhánh.
6. Với flow cũ/new song song, xác định màn nào còn trên menu.
