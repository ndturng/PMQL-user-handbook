# Đăng ký và đánh giá tập huấn

## Mục đích

Nhóm này xử lý nhân sự/giáo viên đăng ký tập huấn, cập nhật hồ sơ, đánh giá kết quả và cấp chứng nhận.

File chính:

- `MODULE/KHOADAOTAO/Dangky_taphuan.ascx.cs`
- `MODULE/KHOADAOTAO/CAPNHAT_Danhgia.ascx.cs`
- `MODULE/KHOADAOTAO/importListDG.aspx.cs`
- `MODULE/KHOADAOTAO/importListGVXS.aspx.cs`
- `MODULE/KHOADAOTAO/InCNHQ.ascx.cs`
- `MODULE/KHOADAOTAO/Add_info.ascx.cs`

## Logic chính

- Đăng ký nhân sự vào khóa tập huấn.
- Cập nhật email, điện thoại, vị trí công tác của `Nhan_su`.
- Ghi điểm/kết quả/nhận xét vào `Danhgia_taphuan`.
- Tạo hoặc kích hoạt `Users` sau khi đạt điều kiện.
- Import danh sách đánh giá/chứng nhận từ file.

## DB liên quan

- `Nhan_su`
- `Giaovien_Taphuan`
- `Danhgia_taphuan`
- `Users`
- `Hocsinh` trong một số side effect tạo/kích hoạt tài khoản học viên liên quan nhân sự.

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp với điểm, nhận xét, id nhân sự, id khóa.
- Cần validate điểm số, trạng thái đạt/không đạt, số chứng nhận.
- Cần tránh tạo trùng `Users` hoặc reset mật khẩu ngoài ý muốn.
- Import cần validate schema, dữ liệu trùng và encoding tiếng Việt.
- Cần transaction khi vừa ghi đánh giá vừa tạo/kích hoạt user.

## Ảnh hưởng module khác

Kết quả đánh giá có thể ảnh hưởng quyền đăng nhập, trạng thái nhân sự, danh sách giáo viên đủ điều kiện và báo cáo đào tạo.
