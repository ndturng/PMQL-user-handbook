# Module Nhân sự (`MODULE/NHANSU`)

## Mục đích

`MODULE/NHANSU` quản lý hồ sơ nhân sự/giáo viên, liên kết nhân sự với tài khoản `Users`, chuyển cơ sở, theo dõi giáo viên tham gia lớp/khóa luyện tập, và thống kê doanh số/doanh thu theo nhân sự.

Module này là dữ liệu nền cho nhiều flow vận hành:

- `LOPHOC` dùng `Nhan_su` để chọn giáo viên trong thời khóa biểu.
- `Event` dùng `Nhan_su`, `Giaovien_Taphuan`, `KhoaTapHuan` để lọc giáo viên tham gia event.
- `KHOADAOTAO` dùng nhân sự/giáo viên cho khóa tập huấn, đánh giá, chứng nhận.
- `USERS` liên kết tài khoản đăng nhập với `Nhan_su`.
- `HOCVIEN`/`KETOAN` dùng `Users`/nhân sự để thống kê tư vấn, đăng ký, doanh thu.

Entry chính:

```text
Default.aspx?mod=nhansu!Danhsach
```

Menu trái cũng trỏ tới:

```text
Default.aspx?mod=nhansu!Danhsach
```

## Trạng thái sử dụng trong app

Module này đang nằm trong flow chính của app.

Bằng chứng:

- `MODULE/menu/menu_left.ascx` có menu nhân sự.
- `MODULE/NHANSU/home.ascx` có entry danh sách nhân sự, luyện tập online, thống kê, doanh thu, lịch sử thao tác.
- `MODULE/LOPHOC/Lophoc_TKB.ascx.cs` load giáo viên từ `Nhan_su`.
- `MODULE/Event/List.ascx.cs` load giáo viên từ `Nhan_su` và khóa tập huấn từ `Giaovien_Taphuan`.
- `MODULE/KHOADAOTAO` thao tác dữ liệu đào tạo/chứng nhận liên quan nhân sự.

## Tài liệu con

- [HoSoNhanSu.md](./HoSoNhanSu.md): danh sách, thêm/sửa/xóa mềm nhân sự, chuyển cơ sở, import.
- [UserPermission.md](./UserPermission.md): liên kết `Nhan_su` với `Users`, username, quyền/template.
- [GiaoVienDaoTao.md](./GiaoVienDaoTao.md): giáo viên, lớp/thời khóa biểu, tập huấn, chứng nhận, luyện tập online.
- [ThongKe.md](./ThongKe.md): thống kê doanh số/doanh thu và lịch sử thao tác.
- [DataModel.md](./DataModel.md): bảng dữ liệu và quan hệ.
- [Issues.md](./Issues.md): rủi ro logic, DB, bảo mật, hiệu suất và việc cần sửa.

## File liên quan

| File | Vai trò |
| --- | --- |
| `MODULE/NHANSU/home.ascx` | Dashboard module. |
| `MODULE/NHANSU/menu_top.ascx` | Menu con module. |
| `MODULE/NHANSU/Danhsach.ascx.cs` | Danh sách nhân sự, filter, xóa mềm, xác nhận chính thức. |
| `MODULE/NHANSU/Add_nhansu.ascx.cs` | Thêm nhân sự và tạo tài khoản `Users`. |
| `MODULE/NHANSU/Add_giaovien_auto.ascx.cs` | Form động thêm/sửa nhân sự, sync thông tin sang `Users`, upload file. |
| `MODULE/NHANSU/Exchange_nhansu.ascx.cs` | Chuyển cơ sở nhân sự và thay giáo viên trong `Lop_TKB`. |
| `MODULE/NHANSU/TeacherJoinClass.ascx.cs` | Danh sách giáo viên tham gia khóa/luyện tập online. |
| `MODULE/NHANSU/Update_JoinClass.ascx.cs` | Thêm giáo viên vào `TeacherJoinClass`. |
| `MODULE/NHANSU/Thongke_doanhSo.ascx.cs` | Thống kê học viên mới, tái ký, hóa đơn theo nhân sự. |
| `MODULE/NHANSU/THONGKE_Doanhthu.ascx.cs` | Thống kê doanh thu theo user/nhân sự. |
| `MODULE/NHANSU/LICHSU_Thaotac.ascx.cs` | Lịch sử thao tác/nhật ký. |
| `MODULE/NHANSU/importList*.aspx.cs` | Import nhân sự/giáo viên. |
| `MODULE/NHANSU/RATE_GV.ascx.cs` | Cập nhật rate/trạng thái tập huấn giáo viên. |
| `MODULE/NHANSU/Class_bygiaovien.ascx.cs` | Danh sách lớp theo giáo viên. |

## Feature map

| Feature | Entry/command | File chính | Bảng chính |
| --- | --- | --- | --- |
| Danh sách nhân sự | `cmd=load` | `Danhsach.ascx.cs` | `Nhan_su`, `Users`, `GCN_Nhansu`, `Danhgia_taphuan` |
| Thêm nhân sự | `cmd=save` | `Add_nhansu.ascx.cs` | `Nhan_su`, `Users`, `Permission_join` |
| Thêm/sửa form auto | post form | `Add_giaovien_auto.ascx.cs` | `Nhan_su`, `Users`, `GCN_Nhansu` |
| Xóa nhân sự | `cmd=delete` | `Danhsach.ascx.cs` | `Nhan_su.enable` |
| Xác nhận chính thức | `cmd=xacnhan` | `Danhsach.ascx.cs` | `Nhan_su.temp` |
| Chuyển cơ sở | `cmd=update` | `Exchange_nhansu.ascx.cs` | `Nhan_su`, `Users`, `Lop_TKB` |
| Giáo viên tham gia khóa/luyện tập | `loadlist`, `cancel` | `TeacherJoinClass.ascx.cs` | `TeacherJoinClass`, `Nhan_su`, `Khoahoc` |
| Thêm giáo viên vào khóa/luyện tập | `save` | `Update_JoinClass.ascx.cs` | `TeacherJoinClass` |
| Thống kê nhân sự | `Loadsum` | `Thongke_doanhSo.ascx.cs` | `Users`, `Nhan_su`, `Hocsinh`, `Lophoc_join`, `Dangky_group` |
| Thống kê doanh thu | `Load_list` | `THONGKE_Doanhthu.ascx.cs` | `Users`, `Dangky_group`, `Hocphi`, `Dangky_thukhac` |

## Permission key chính

| Permission | Ý nghĩa quan sát được |
| --- | --- |
| `04NS` / `04NS ` | Danh sách/thêm/sửa/xóa nhân sự; có nơi có khoảng trắng dư. |
| `04NSLoG` | Luyện tập online hoặc lịch sử thao tác theo home. |
| `04TK` | Thống kê nhân sự. |
| `04TKDT` | Thống kê doanh thu nhân sự. |
| `17KTH` | Thêm giáo viên vào `TeacherJoinClass`. |

## Nhận xét nhanh

`NHANSU` vừa là module hồ sơ nhân sự, vừa có side effect sang `Users`, `Lop_TKB`, `TeacherJoinClass` và dữ liệu đào tạo. Khi sửa module này, không chỉ kiểm tra màn danh sách nhân sự mà cần kiểm tra đăng nhập/phân quyền, thời khóa biểu lớp, event giáo viên và khóa đào tạo.
