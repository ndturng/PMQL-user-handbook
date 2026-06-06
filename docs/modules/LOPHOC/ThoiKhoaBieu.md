# Thời khóa biểu

## Feature

Feature thời khóa biểu gồm hai phần:

- Cấu hình lịch học cho một lớp.
- Xem lịch/statistics theo tuần/ngày để vận hành lớp, phòng và giáo viên.

## Trạng thái trong flow chính

Có dùng trong flow chính.

- Cấu hình lịch lớp: link từ danh sách lớp tới `Lophoc_TKB`.
- Xem lịch tuần: `lophoc!TKB_Statics`.
- Permission chính: `03TKB`.

## File/code liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/LOPHOC/Lophoc_TKB.ascx.cs` | Thêm/sửa lịch học của một lớp. |
| `MODULE/LOPHOC/TKB_Statics.ascx.cs` | Xem lịch tuần/thống kê lịch. |
| `App_Code/BO/lophoc.cs` | Helper kiểm tra ngày hợp lệ/ngày rủi ro. |
| `MODULE/LOPHOC/Phonghoc.ascx.cs` | Danh mục phòng học dùng trong lịch. |
| `MODULE/LOPHOC/Print_TKB/*` | In thời khóa biểu. |

## Logic cấu hình lịch lớp

`Lophoc_TKB.ascx.cs` load thông tin lớp, danh sách giáo viên và phòng học, sau đó cho người dùng chọn các ngày học trong tuần.

Khi lưu:

1. Xóa toàn bộ lịch cũ trong `Lop_TKB` theo `idlop`.
2. Insert lại từng dòng lịch theo weekday được chọn.
3. Mỗi dòng có `idweek`, `idlop`, `fromdate`, `todate`, `idgiaovien`, `idphonghoc`, `iduser`.

## Logic xem lịch tuần

`TKB_Statics.ascx.cs` nhận khoảng ngày, sau đó:

1. Lấy các lớp thuộc chi nhánh và còn nằm trong khoảng thời gian học.
2. Join với `Lop_TKB`, `Nhan_su`, `PhongHoc`.
3. Render lịch theo ngày/tuần.
4. Tính số học viên active trong ngày bằng `Date_counthv`.
5. Liệt kê khóa học trong lớp bằng `List_khoahoc`.

## DB tương tác

| Bảng | Vai trò |
| --- | --- |
| `Lop_TKB` | Bảng lịch học của từng lớp. |
| `Lophoc` | Thông tin lớp, ngày bắt đầu/kết thúc. |
| `Nhan_su` | Giáo viên đứng lớp. |
| `PhongHoc` | Phòng học. |
| `Lophoc_join` | Tính học viên active theo ngày. |
| `Khoahoc` | Hiển thị khóa học trong lịch. |
| `TKB_Risk` | Ngày nghỉ/ngày rủi ro. |

## Tương tác với module khác

- `PhongHoc` cung cấp phòng học để chọn lịch.
- `Nhan_su` cung cấp giáo viên.
- `HOCVIEN`/`Lophoc_join` cung cấp sĩ số active theo ngày.
- Điểm danh và kết quả theo ngày phụ thuộc vào lịch để biết ngày nào có buổi học.
- Bảo lưu/ngày nghỉ dùng lịch tuần để tính ngày bù.

## Overlap/xung đột

- `LOPHOC.TKB_Check_hopledate` chỉ kiểm tra weekday, chưa thấy dùng đầy đủ `startdate` và `idchinhanh`; có thể làm các màn hình hiểu sai ngày học hợp lệ.
- `TKB_Statics.ascx.cs` có hàm kiểm tra `TKB_Risk`, nhưng flow chọn ngày ở một số nơi vẫn dùng helper khác chưa loại ngày nghỉ rõ ràng.
- Chưa thấy kiểm tra xung đột giáo viên/phòng khi hai lớp cùng giờ.

## Vấn đề cần sửa

- Lưu lịch bằng cách delete toàn bộ rồi insert lại, **không thấy transaction**; nếu lỗi giữa chừng có thể mất lịch.
- Chưa có unique constraint rõ ràng để tránh trùng `idlop` + `idweek` + giờ.
- Chưa chặn trùng giáo viên/phòng theo cùng khung giờ.
- **SQL injection do truy vấn SQL nối chuỗi trực tiếp**.
- Cần chuẩn hóa cách xử lý `TKB_Risk` trong tất cả màn hình dùng lịch.
