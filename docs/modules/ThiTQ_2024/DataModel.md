# Data model Thi TQ 2024

## Bảng chính

Cần xác nhận module này dùng bảng riêng theo năm hay dùng chung các bảng của `ThiTQ`. Footprint file cho thấy domain giống `ThiTQ`:

- `ThiTQ`
- `CauhoiThiTQ`
- `ThiTQ_RankNgheOnline`
- `Hocsinh`
- `Chinhanh`
- `ThuChi` / `Thu_Chi`

## Issue DB

- Nếu dùng chung bảng với `ThiTQ`, cần có trường năm/kỳ thi rõ ràng.
- Nếu dùng bảng riêng, cần document mapping và báo cáo không được query nhầm.
- Cần chuẩn hóa trạng thái thanh toán/trạng thái thi.
- Cần audit thay đổi kết quả/chứng nhận.
