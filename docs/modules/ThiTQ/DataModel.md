# Data model module Thi TQ

## Bảng chính

| Bảng | Vai trò |
| --- | --- |
| `ThiTQ` | Thí sinh/đăng ký/kết quả thi TQ. |
| `CauhoiThiTQ` | Câu hỏi thi theo năm/vòng/bảng/khu vực. |
| `ThiTQ_RankNgheOnline` | Cấu hình xếp hạng. |
| `Hocsinh` | Học viên/thí sinh liên quan. |
| `Chinhanh` | Chi nhánh đăng ký. |
| `ThuChi` / `Thu_Chi` | Phiếu thu/thanh toán phí thi. |

## Quan hệ quan trọng

- `ThiTQ.idchinhanh` -> `Chinhanh.id`
- `ThiTQ.idhocvien` hoặc trường tương đương -> `Hocsinh.id`
- Phiếu thu/thanh toán tham chiếu đăng ký thi hoặc chi nhánh.

## Issue DB

- Cần xác nhận khóa chính/liên kết giữa đăng ký nhóm và thí sinh.
- Cần ràng buộc theo năm/kỳ thi để tránh ghi nhầm dữ liệu năm cũ.
- Cần chuẩn hóa trạng thái thanh toán/trạng thái thi.
- Cần audit thay đổi điểm/kết quả/chứng nhận.
