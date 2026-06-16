# Website HD — Hướng dẫn vận hành

Thư mục này là **nguồn duy nhất** của website. Mọi cập nhật chỉ làm ở đây.

## Cấu trúc

```
website/
├── index.html      ← Báo cáo ngày  (trang chủ, mở ở URL "/")
├── cashflow.html   ← Dòng tiền / Chứng từ
├── .nojekyll       ← file rỗng, GIỮ NGUYÊN, đừng xóa
└── README.md       ← file này
```

> ⚠️ Không đổi tên `index.html` (đây là trang chủ). Không xóa `.nojekyll`.

## Thông tin cố định

| Mục | Giá trị |
|---|---|
| Tên repo (GÕ CHÍNH XÁC) | `thaophuongbui93-pixel.github.io` |
| Loại | User site (1 tài khoản chỉ có 1 repo loại này) |
| Branch | `main` |
| Pages – Source | Deploy from a branch → `main` / `(root)` |

## URL chuẩn (cố định lâu dài)

- Báo cáo ngày: **https://thaophuongbui93-pixel.github.io/**
- Dòng tiền: **https://thaophuongbui93-pixel.github.io/cashflow.html**

URL này không bao giờ đổi chừng nào tên repo và username còn giữ nguyên.

---

## A. Cài đặt 1 lần (lần đầu đưa lên GitHub)

Vì file đã có sẵn trong máy nên dùng cách **Publish** (không clone):

1. Mở **GitHub Desktop** → menu **File → Add local repository** → chọn thư mục `website` này.
2. GitHub Desktop báo "không phải repo Git" → bấm dòng **"create a repository"** → **Create repository**.
3. Bấm nút **Publish repository** (góc trên bên phải):
   - **Name:** gõ chính xác `thaophuongbui93-pixel.github.io`
   - **Bỏ tick** "Keep this code private" (Pages miễn phí cần repo **Public**)
   - Bấm **Publish repository**.
4. Vào **GitHub.com → repo vừa tạo → Settings → Pages**:
   - Source: **Deploy from a branch** → Branch **main** / **(root)** → **Save**.
5. Đợi ~1 phút, mở **https://thaophuongbui93-pixel.github.io/** để kiểm tra.

Sau khi site mới chạy ổn → **xóa repo cũ tên `index.html`** trên GitHub để khỏi nhầm lẫn.

---

## B. Quy trình cập nhật về sau — 3 bước

1. **Claude sửa file** trong thư mục `website/` này.
2. Mở **GitHub Desktop** → gõ mô tả ngắn ở ô **Summary** → bấm **Commit to main**.
3. Bấm **Push origin** → khoảng 1 phút sau website tự cập nhật.

Một luồng duy nhất. Không upload tay, không trùng file.

---

## Lưu ý OneDrive

Thư mục `website` nằm trong OneDrive. Để tránh lỗi Git do file bị để "trên mây":
chuột phải thư mục `website` → chọn **"Luôn giữ trên thiết bị này"** (Always keep on this device). Làm 1 lần.
