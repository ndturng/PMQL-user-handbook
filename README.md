# PMQL User Handbook

Tài liệu hướng dẫn sử dụng hệ thống PMQL dành cho nhân viên nội bộ.

🌐 **Website:** https://ndturng.github.io/PMQL-user-handbook/

---

## Cấu trúc thư mục

```
PMQL-user-handbook/
│
├── mkdocs.yml                        # Cấu hình MkDocs (menu, theme, ...)
├── README.md                         # File này
│
├── .github/
│   └── workflows/
│       └── deploy.yml                # Tự động build & deploy khi push
│
└── docs/
    └── user-handbook/
        ├── assets/                   # Ảnh minh họa, phân theo module
        │   ├── EmailSms/
        │   ├── Event/
        │   └── ...
        └── modules/                  # Nội dung tài liệu (.md)
            ├── index.md              # Trang chủ
            ├── CoSo/
            ├── DISC/
            ├── EmailSms/
            └── ...
```

---

## Hướng dẫn deploy lần đầu

### Bước 1: Tạo repo mới trên GitHub

1. Đăng nhập GitHub → nhấn **New repository**
2. Đặt tên repo (ví dụ: `PMQL-user-handbook`)
3. Chọn **Public**
4. Nhấn **Create repository**

### Bước 2: Push code lên repo

```bash
git init
git add .
git commit -m "init commit"
git branch -M main
git remote add origin https://github.com/<username>/<repo-name>.git
git push -u origin main
```

### Bước 3: Bật GitHub Pages

1. Vào repo → **Settings** → **Pages**
2. Phần **Source** → chọn **Deploy from a branch**
3. Phần **Branch** → chọn **`gh-pages`** → **`/ (root)`**
4. Nhấn **Save**

> **Lưu ý:** Branch `gh-pages` sẽ được tạo tự động sau lần push đầu tiên.
> Nếu chưa thấy trong dropdown, chờ ~2 phút để GitHub Actions chạy xong rồi refresh lại.

### Bước 4: Kiểm tra deploy

1. Vào tab **Actions** của repo
2. Chờ workflow **"Deploy MkDocs to GitHub Pages"** chạy xong (dấu ✅ xanh, ~1-2 phút)
3. Truy cập website tại:

```
https://<username>.github.io/<repo-name>/
```

---

## Hướng dẫn cập nhật nội dung

Sau khi deploy lần đầu, mỗi khi cần cập nhật tài liệu chỉ cần:

```bash
# 1. Sửa file .md trong docs/user-handbook/modules/
# 2. Push lên GitHub
git add .
git commit -m "mô tả thay đổi"
git push
```

GitHub Actions sẽ tự động build và deploy lại website trong ~1-2 phút.

---

## Hướng dẫn thêm module mới

**1. Tạo thư mục và file .md:**
```
docs/user-handbook/modules/TenModule/
    Overview.md
    TieuDe1.md
    TieuDe2.md
    LuuYVanHanh.md
```

**2. Tạo thư mục ảnh tương ứng (nếu có):**
```
docs/user-handbook/assets/TenModule/
    anh-minh-hoa.png
```

**3. Thêm vào `mkdocs.yml`:**
```yaml
nav:
  ...
  - Tên Module:
    - Tổng quan: TenModule/Overview.md
    - Tiêu đề 1: TenModule/TieuDe1.md
    - Tiêu đề 2: TenModule/TieuDe2.md
    - Lưu ý vận hành: TenModule/LuuYVanHanh.md
```

**4. Push lên GitHub → website tự cập nhật.**

---

## Lưu ý về đường dẫn ảnh

Ảnh được lưu trong `docs/user-handbook/assets/`. Trong file `.md`, dùng đường dẫn tương đối:

```markdown
![Mô tả ảnh](../../assets/TenModule/ten-anh.png)
```

Workflow deploy sẽ tự động copy thư mục `assets/` vào đúng vị trí khi build.

---

## Giới hạn sử dụng (GitHub Free)

| | Giới hạn |
|---|---|
| Số lần push/deploy | Không giới hạn |
| Số lượt truy cập | Không giới hạn |
| Dung lượng repo | Khuyến nghị dưới 1GB |
| Băng thông | 100GB/tháng |