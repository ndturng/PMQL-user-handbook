# Module Lớp học (`MODULE/LOPHOC`)

## Mục tiêu

Module `LOPHOC` quản lý vòng đời vận hành của lớp học sau khi học viên đã đăng ký khóa học: tạo lớp, xếp học viên vào lớp, cấu hình thời khóa biểu, theo dõi sĩ số, điểm danh, nhập kết quả cuối khóa, gửi email kết quả và xem thống kê.

Đây là module nằm trong flow chính của app. Menu trái trỏ vào `Default.aspx?mod=lophoc!home`, sau đó `home.ascx` và `Menu_top.ascx` dẫn tới các feature con bằng các permission key như `03LH`, `03PH`, `03KQN`, `03CKGK`, `03TKB`, `03TK`.

## Trạng thái sử dụng trong flow chính

| Feature | Có trong flow chính? | Entry chính | Ghi chú |
| --- | --- | --- | --- |
| Quản lý lớp học | Có | `lophoc!lophoc` | Danh sách lớp, thêm/sửa/xóa mềm lớp. |
| Học viên trong lớp | Có | Link từ danh sách lớp | Xem và gỡ học viên khỏi lớp. |
| Thời khóa biểu | Có | `lophoc!TKB_Statics`, `lophoc!Lophoc_TKB` | Vừa là cấu hình lịch lớp, vừa là màn hình thống kê lịch tuần. |
| Phòng học | Có | `lophoc!phonghoc` | Dữ liệu phòng được dùng khi xếp lịch lớp. |
| Điểm danh/kết quả ngày | Có | `lophoc!diemdanh` | Có cả bảng `Lophoc_diemdanh` và bảng động `Diemdanh_style*`. |
| Điểm/kết quả cuối khóa | Có | `lophoc!nhapdiem`, `lophoc!Loc_ketqua` | Có bảng `BangDiem` và bảng động `Ketqua_style*`. |
| Thống kê lớp | Có | `lophoc!thongke` | Theo dõi sĩ số, lớp sắp hết/kết thúc. |
| In ấn | Có, phụ trợ | Các thư mục `Print*` | Phục vụ in lịch, điểm danh, kết quả. |

## Tài liệu con

- [DanhSachLop.md](./DanhSachLop.md): danh sách lớp, thêm/sửa lớp, xóa mềm lớp.
- [HocVienTrongLop.md](./HocVienTrongLop.md): danh sách học viên trong lớp, trạng thái còn học/hết hạn, thao tác gỡ khỏi lớp.
- [ThoiKhoaBieu.md](./ThoiKhoaBieu.md): cấu hình lịch học, lịch tuần, phòng học/giáo viên, ngày nghỉ.
- [DiemDanhKetQuaNgay.md](./DiemDanhKetQuaNgay.md): điểm danh và kết quả theo ngày.
- [KetQuaCuoiKhoa.md](./KetQuaCuoiKhoa.md): nhập điểm cuối khóa, lọc kết quả, gửi email.
- [PhongHoc.md](./PhongHoc.md): danh mục phòng học.
- [DataModel.md](./DataModel.md): các bảng liên quan và quan hệ dữ liệu.
- [Issues.md](./Issues.md): rủi ro logic, DB, bảo mật, hiệu suất và việc cần sửa.

## Các module liên quan

| Module/feature | Quan hệ với `LOPHOC` |
| --- | --- |
| `HOCVIEN` | Hồ sơ học viên, đăng ký khóa học, bảo lưu/gia hạn ảnh hưởng trực tiếp đến `Lophoc_join`, sĩ số, ngày kết thúc lớp. |
| `Sukien` / `Event` | Có overlap về khái niệm ghi danh/sự kiện, nhưng không dùng chung lớp học; tránh nhầm với event registration. |
| `ChuongTrinh` | Cấu hình chương trình quyết định bảng động dùng để nhập điểm danh và kết quả (`modulediemdanh`, `modulediem`). |
| `Nhan_su` | Giáo viên được chọn khi cấu hình thời khóa biểu. |
| `PhongHoc` | Phòng học được chọn trong lịch lớp. |
| `TKB_Risk` | Ngày nghỉ/ngày rủi ro ảnh hưởng đến lịch và thời lượng học, nhưng logic hiện không thống nhất giữa các màn hình. |
| `Baoluu_khoahoc`, `Giahan_khoahoc` | Bảo lưu/gia hạn làm thay đổi cách tính ngày kết thúc học viên trong lớp. |

## Nhận xét nhanh

`LOPHOC` là module lõi và có mức độ phụ thuộc cao. Không nên sửa riêng một màn hình mà không kiểm tra tác động đến `HOCVIEN`, `ChuongTrinh`, `Lophoc_join`, `Lop_TKB`, điểm danh và kết quả cuối khóa.

Rủi ro lớn nhất hiện tại là logic ngày kết thúc học viên/lớp bị lặp và không đồng nhất ở nhiều file; SQL injection do truy vấn SQL nối chuỗi trực tiếp xuất hiện nhiều; một số permission key có khoảng trắng dư; dữ liệu điểm danh/kết quả tồn tại song song giữa bảng cố định và bảng động.
