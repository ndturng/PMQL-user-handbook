# Data model module kho

## Bảng chính

| Bảng | Vai trò |
| --- | --- |
| `KHO` | Tồn kho theo vật tư/chi nhánh. |
| `KHO_Chitietphieu` | Chi tiết nhập/xuất/điều chỉnh kho. |
| Bảng đơn hàng kho | Đơn đặt vật tư. |
| Bảng chi tiết đơn hàng kho | Vật tư, số lượng, đơn giá theo đơn. |
| `VatTu`/danh mục vật tư | Thông tin vật tư. |
| `Chinhanh` | Chi nhánh sở hữu tồn kho/đơn hàng. |
| `ThuChi` / `Thu_Chi` | Phiếu kế toán phát sinh từ đơn hàng. |

## Quan hệ quan trọng

- `KHO.idvattu` -> danh mục vật tư.
- `KHO.idChinhanh` -> `Chinhanh.id`.
- `KHO_Chitietphieu.idvattu` -> danh mục vật tư.
- `KHO_Chitietphieu.iddonhang` -> đơn hàng kho.

## Issue DB

- Cần xác định tên bảng đơn hàng chính xác và trạng thái đơn hàng.
- Cần transaction khi insert chi tiết phiếu và update tồn.
- Cần ràng buộc không để trùng dòng tồn kho cùng `idvattu` + `idChinhanh` nếu nghiệp vụ yêu cầu một dòng duy nhất.
- Cần lưu rõ đơn vị gốc, đơn vị quy đổi và hệ số để báo cáo nhất quán.
