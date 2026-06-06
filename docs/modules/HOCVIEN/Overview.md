# Module: HOCVIEN

## Mục đích

`MODULE/HOCVIEN` là nhóm nghiệp vụ trung tâm cho học viên. Module này không chỉ lưu hồ sơ học viên mà còn là điểm đi vào cho nhiều flow khác:

- Danh sách/tìm kiếm/lọc học viên.
- Thêm mới, sửa hồ sơ, xem chi tiết học viên.
- Đăng ký khóa học, lập phiếu đăng ký, ghi nhận khoản thu khác/vật tư đi kèm.
- Theo dõi lịch sử đăng ký, trạng thái đang học, chờ xếp lớp, kết thúc, nghỉ học, bảo lưu.
- Bảo lưu học phí/khóa học và thống kê bảo lưu.
- Import học viên, thi TQ, nhập điểm, cấp giấy chứng nhận, in ấn.
- Đăng ký học viên tham gia event qua `MODULE/Event`.

Entry chính:

```text
Default.aspx?mod=hocvien!home
```

## Trạng thái sử dụng trong app

Module này **đang là flow chính** cho dữ liệu học viên.

Bằng chứng trong code:

- `MODULE/menu/menu_left.ascx` trỏ menu Học viên tới `Default.aspx?mod=hocvien!home`.
- `MODULE/HOCVIEN/home.ascx` là dashboard học viên, link tới danh sách, lịch sử đăng ký, thống kê, bảo lưu, gần kết khóa, cấp giấy chứng nhận, sinh nhật, khôi phục học viên.
- `MODULE/HOCVIEN/Menu_top.ascx` là menu phụ dùng trong nhiều màn hình học viên.
- `MODULE/HOCVIEN/Danhsach.ascx.cs` là list chính và có các hành động quan trọng như đăng ký khóa học, sửa/xóa học viên, chuyển cơ sở, đăng ký event.

## Cách đọc bộ tài liệu này

Do `MODULE/HOCVIEN` rất lớn, tài liệu được tách theo feature:

| File | Nội dung |
| --- | --- |
| `Overview.md` | Tổng quan, flow chính, overlap, bảng dữ liệu chính |
| `Danhsach.md` | Danh sách học viên, trạng thái học viên, action trên list |
| `HoSoHocVien.md` | Thêm/sửa/xem hồ sơ học viên |
| `DangKyKhoaHoc.md` | Phiếu đăng ký khóa học, đăng ký group, học phí, vật tư |
| `BaoLuu.md` | Bảo lưu học phí/khóa học và thống kê bảo lưu |
| `ThongKeBaoCao.md` | Thống kê, danh sách đặc thù, thi TQ, giấy chứng nhận, in ấn |
| `EventRegistration.md` | Tương tác với module Event |
| `DataModel.md` | Bảng dữ liệu liên quan |
| `Issues.md` | Vấn đề logic, bảo mật, hiệu suất cần sửa |

## File/feature map

| Nhóm | File tiêu biểu | Vai trò |
| --- | --- | --- |
| Dashboard/menu | `home.ascx`, `Menu_top.ascx` | Entry và điều hướng module |
| Danh sách | `Danhsach.ascx`, `Danhsach.ascx.cs`, `Search.ascx` | List, search, filter, action học viên |
| Hồ sơ | `Addnew_auto.ascx.cs`, `View_Detail_auto.ascx.cs`, `edit_hocvien.ascx` | Thêm/sửa/xem hồ sơ |
| Đăng ký | `Dangky_group.ascx.cs`, `DANGKY/dangky_togroup.aspx.cs`, `DANGKY/Phieuthu_hocphi.aspx.cs` | Phiếu đăng ký khóa học, thu học phí |
| Bảo lưu | `Baoluu_hocphi.ascx.cs`, `Baoluu_kq.ascx.cs`, `Thongke_baoluu.ascx.cs` | Bảo lưu học phí/khóa học |
| Lịch sử | `History-registry.ascx.cs`, `hocvien_history.ascx.cs` | Lịch sử đăng ký/học tập |
| Danh sách đặc thù | `Danhsach_14days`, `Danhsach_nghihoc`, `Danhsach_daxoa`, `Danhsach_CNKK` | Gần kết khóa, nghỉ học, đã xóa, giấy chứng nhận |
| Import | `importList*.aspx.cs` | Import học viên/dữ liệu thi |
| Thi TQ | `Danhsach_thitq`, `Nhapdiem_thitq`, `Thongke_thitq`, `Xephang_thitq` | Dữ liệu thi TQ |
| In ấn | `Print/`, `Printcu/` | Phiếu, chứng chỉ, sinh nhật |

## Module/feature overlap hoặc xung đột

### Với `MODULE/Event`

`Danhsach.ascx.cs` render action `btn-eventRegistration` cho học viên. Script trong `Danhsach.ascx` gọi:

```text
Default.aspx?mod=event!manage&cmd=load_register
Default.aspx?mod=event!manage&cmd=save
```

Rủi ro: `event!manage` là flow đăng ký event cũ hơn flow chính trong `MODULE/Event/List.ascx.cs`, chỉ xử lý học viên và không đồng bộ đủ rule event có phí/`ConsumedAt`.

### Với `MODULE/LOPHOC`

HOCVIEN đọc/ghi `Lophoc_join` để xác định học viên đang học, bảo lưu, kết thúc sớm, gia hạn, chờ xếp lớp. Các thay đổi ở đây ảnh hưởng trực tiếp danh sách lớp và trạng thái học viên.

### Với `MODULE/KHOAHOC`

Đăng ký khóa học ghi `Dangky.IDkhoahoc` và phụ thuộc học phí/level/khóa học. Việc xóa/hủy khóa học hoặc thay đổi khóa học ảnh hưởng học phí, vật tư và trạng thái học viên.

### Với `MODULE/KETOAN`

Phiếu đăng ký và học phí liên quan `ThuChi`, `Hocphi`, `Dangky_thukhac`, `Hocsinh_voucher`. Một số màn vừa đọc bảng cũ `Hocphi`, vừa đọc bảng mới `ThuChi`.

### Với kho/vật tư

Đăng ký khóa học có thể insert vật tư đi kèm vào `KHO_Chitietphieu`, dựa trên cấu hình `Khoahoc_kho` và `donhang_setting`.

## Phân quyền chính

| Permission | Ý nghĩa quan sát được |
| --- | --- |
| `01HV` | Xem/thêm/sửa/xóa học viên |
| `01DKKH` | Lịch sử/phiếu đăng ký khóa học |
| `01NTK` | Bảo lưu/nhóm nghiệp vụ liên quan tài khoản/học phí |
| `01TK` | Thống kê học viên |
| `01HVV` | Học viên nghỉ học/vắng |
| `0114N` | Danh sách gần kết khóa/14 ngày |
| `01GCN` / `01GCN ` | Cấp giấy chứng nhận, có chỗ dùng key có dấu cách |
| `01TDV ` | Test đầu vào, có dấu cách trong key |
| `01THP` | Phiếu thu học phí |
| `05PTC` | Tạo/thanhtoan phiếu thu từ popup phí cơ sở |

## Bảng dữ liệu chính

Chi tiết ở `DataModel.md`. Các bảng trọng yếu:

- `Hocsinh`
- `Dangky_group`
- `Dangky`
- `Lophoc_join`
- `Baoluu_khoahoc`
- `Hocsinh_voucher`
- `Hocphi`
- `ThuChi`
- `Dangky_thukhac`
- `KHO_Chitietphieu`
- `Nhansu_Log`
- `Users`, `ChiNhanh`
- `Khoahoc`, `Chuongtrinh`, `Lophoc`

## Nhận định cấp module

`HOCVIEN` là nguồn dữ liệu cho nhiều module khác. Thay đổi nhỏ trong trạng thái học viên hoặc phiếu đăng ký có thể ảnh hưởng tới lớp học, học phí, báo cáo, event, kho và in ấn.

Ưu tiên khi sửa module này:

1. Xác định flow đang sửa là hồ sơ, đăng ký khóa, bảo lưu, thống kê hay event.
2. Kiểm tra bảng nào là source of truth cho trạng thái đó.
3. Kiểm tra có bảng legacy song song không, ví dụ `Hocphi` và `ThuChi`.
4. Không sửa trạng thái học viên chỉ ở UI; cần kiểm tra `Dangky`, `Lophoc_join`, `Baoluu_khoahoc`.
5. Với thao tác tài chính/hủy đăng ký/bảo lưu, cần transaction hoặc ít nhất kiểm tra các bảng phụ bị ảnh hưởng.
