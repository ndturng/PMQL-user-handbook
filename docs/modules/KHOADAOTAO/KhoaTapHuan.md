# Khóa tập huấn

## Mục đích

Nhóm này tạo, sửa, kết thúc và xóa khóa tập huấn.

File chính:

- `MODULE/KHOADAOTAO/Danhsach.ascx.cs`
- `MODULE/KHOADAOTAO/Add_Khoataphuan.ascx.cs`
- `MODULE/KHOADAOTAO/Add_nhansu.ascx.cs`
- `MODULE/KHOADAOTAO/CAPNHAT_Edit.ascx.cs`
- `MODULE/KHOADAOTAO/CAPNHAT_Ketthuc.ascx.cs`

## Logic chính

- Tạo khóa tập huấn với tên, thời gian, số lượng, phí, hạn đăng ký.
- Tạo notification/feed cho chi nhánh khi có lịch tập huấn.
- Sửa thông tin khóa.
- Xóa hoặc disable khóa.
- Kết thúc khóa để khóa thao tác/đánh giá tùy logic.

## DB liên quan

- `KhoaTapHuan`
- `Feed_news`
- `Feed_news_users`
- `Giaovien_Taphuan`

## Vấn đề cần chú ý

- Cần kiểm tra SQL injection do truy vấn SQL nối chuỗi trực tiếp từ tên khóa, ngày, phí, ghi chú.
- Cần validate ngày bắt đầu/kết thúc/hạn đăng ký.
- Cần transaction khi tạo khóa và tạo feed/notification cho nhiều chi nhánh.
- Xóa khóa cần kiểm tra đăng ký/đánh giá đã phát sinh.
