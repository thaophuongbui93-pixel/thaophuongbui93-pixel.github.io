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

## ⭐ HẰNG NGÀY — SOP cập nhật bằng Claude (không cần biết code)

> **Dùng phần này mỗi ngày.** Chỉ cần copy prompt, **đổi ngày**, dán cho Claude (Cowork đã kết nối thư mục **HD**). Chi tiết trường & vị trí dữ liệu xem **mục B** bên dưới.
>
> Trong các prompt mẫu: thay **17/06/2026** và mã thư mục **1706** bằng ngày thực tế của bạn.

**Quy trình 1 hình:**

```
Nguồn trong "Báo cáo ngày"          →  Claude cập nhật            →  Bạn đẩy lên web
──────────────────────────────────     ─────────────────────         ──────────────────
T6/1706  (sổ quỹ + phiếu)           →  cashflow.html  (Prompt 1)  →  GitHub Desktop:
Ảnh biến động số dư/1706 (SMS NH)   →  index.html     (Prompt 2)     Commit → Push
                                       kiểm khớp       (Prompt 3)  →  ~1 phút: web có số mới
```

**Chuẩn bị (1 phút):** xác định ngày + mã thư mục (17/06 → `1706`); kiểm tra đã có 2 thư mục nguồn `Báo cáo ngày/T6/1706/` và `Báo cáo ngày/Ảnh biến động số dư/1706/`.

---

### Bước 1 — Prompt cập nhật `cashflow.html`

Copy, đổi ngày, dán cho Claude:

```
Cập nhật website/cashflow.html cho ngày 17/06/2026 theo mục B của website/README.md.
Đọc dữ liệu từ 2 thư mục:
- "Báo cáo ngày/T6/1706" (ảnh sổ quỹ + phiếu thu/chi)
- "Báo cáo ngày/Ảnh biến động số dư/1706" (ảnh SMS ngân hàng)
Việc cần làm:
1) Thêm 1 object cho ngày 2026-06-17 vào mảng NGAY: phân loại đủ thuKH, thuVay, thuKhac,
   chiVT, chiLai, chiDT, chiKhac, fund (tồn quỹ cuối ngày), bank (số dư BVBank);
   nếu có số dư nhiều ngân hàng thì thêm banks[].
2) Nhúng ảnh phiếu thu/chi vào CT["2026-06-17"] dạng base64, ghi rõ loại (Chi/Thu/QT) và số tiền.
3) Đảm bảo "tổng chi từ chứng từ = tổng chi trong ngày" (ô ghi chú hiện ✓).
Chỉ thêm dữ liệu ngày 17/06, không sửa bố cục/chức năng khác. Báo lại bảng số đã nhập.
```

### Bước 2 — Prompt cập nhật các chỉ số tổng hợp trên `index.html`

```
Cập nhật website/index.html cho ngày 17/06/2026 KHỚP ĐÚNG với cashflow.html vừa cập nhật.
Việc cần làm:
1) Append đúng 1 giá trị/ngày cho các mảng: labels ('17/06'), dayNum (17), fund, thu, chi,
   thu_op, thu_fin, thu_oth, chi_op, chi_fin, chi_cap, chi_oth, bankEOD.
2) Thêm các giao dịch BVBank trong ngày vào mảng bank:
   [17,'17/06 HH:MM','nội dung GD', ±số tiền, số dư sau GD].
3) Cập nhật số dư VPBank/TPBank/Vietcombank của ngày mới vào otherBanksByDay (BVBank đã có ở bankEOD)
   để KPI tổng, mục tổng hợp & bảng số dư 4 NH theo ngày tự cập nhật.
4) Cập nhật text "Dữ liệu 01–17/06" (ô góc trên + footer).
Đảm bảo MỌI mảng có cùng số phần tử. Chỉ thêm/cập nhật dữ liệu, không sửa bố cục/chức năng khác.
Báo lại tổng số dư ngân hàng mới.
```

> 💡 **Muốn nhanh hơn?** Gộp Bước 1 + 2 bằng 1 prompt:
> *"Cập nhật website cho ngày 17/06/2026 theo mục B của README: đọc 2 thư mục nguồn, cập nhật cả cashflow.html và index.html cho khớp nhau, nhúng ảnh chứng từ, rồi kiểm khớp và báo lại."*

### Bước 3 — Prompt kiểm tra dữ liệu sau khi cập nhật

```
Kiểm chứng dữ liệu ngày 17/06 trên website/index.html và website/cashflow.html:
- Đối chiếu từng số index ↔ cashflow: thu_op=thuKH, thu_fin=thuVay, thu_oth=thuKhac,
  chi_op=chiVT, chi_fin=chiLai, chi_cap=chiDT, chi_oth=chiKhac, fund=fund, bankEOD=bank.
- fund ngày 17/06 = TỒN QUỸ CÔNG TY trên ảnh sổ quỹ.
- Tổng banksEOD16 = KPI "Số dư ngân hàng".
- Mọi mảng trong index.html cùng số phần tử.
Báo bảng so sánh và chỉ rõ chỗ lệch (nếu có).
```

**Tự xem nhanh (không cần Claude):** mở `cashflow.html` (bấm đúp) → chọn 17/06 → ô ghi chú phải hiện **✓** (không ⚠). Mở `index.html` → chọn 17/06 → KPI và biểu đồ có số mới.

### Bước 4 — Commit & Publish lên GitHub Pages

1. Mở **GitHub Desktop** (thư mục `website` đang hiện các file vừa đổi).
2. Ô **Summary** gõ: `Cập nhật ngày 17/06`.
3. Bấm **Commit to main** → bấm **Push origin**.
4. Đợi ~1 phút → mở **https://thaophuongbui93-pixel.github.io/** kiểm tra. (Chi tiết: mục C.)

### Bước 5 — ✅ Checklist cuối (số liệu website = báo cáo nguồn)

- [ ] `fund` (tồn quỹ) ngày mới = **TỒN QUỸ CÔNG TY** trên ảnh sổ quỹ.
- [ ] 7 nhóm thu/chi của index khớp cashflow (Prompt kiểm tra báo ✓).
- [ ] `cashflow.html` chọn ngày mới → ghi chú hiện **✓** (không ⚠).
- [ ] Số dư BVBank = số trên ảnh SMS = dòng cuối mảng `bank` = BVBank trong `banksEOD16`.
- [ ] KPI "Số dư ngân hàng" (TỔNG) = cộng đúng số dư các ngân hàng.
- [ ] Mọi mảng trong `index.html` cùng số phần tử (= số ngày).
- [ ] Text "Dữ liệu 01–DD/06" đã đổi sang ngày mới nhất.
- [ ] Đã **Commit + Push**; mở URL thật thấy số mới sau ~1 phút.

---

## B. Chi tiết kỹ thuật — trường dữ liệu & vị trí cập nhật

Mỗi ngày lấy số liệu từ thư mục `Báo cáo ngày/` rồi cập nhật vào 2 file trong `website/`. Cả 2 file đều **tự chứa** — số liệu nằm trong các **mảng JavaScript** bên trong file, **không tự đồng bộ với Excel** → phải cập nhật theo hướng dẫn dưới.

### B1. Nguồn dữ liệu cần lấy

| Nguồn | Vị trí thư mục | Cung cấp số gì |
|---|---|---|
| Sổ quỹ + phiếu thu/chi trong ngày | `Báo cáo ngày/T6/<DDMM>/` (vd `T6/1706/`) | 7 nhóm thu/chi + **TỒN QUỸ CÔNG TY** cuối ngày + ảnh chứng từ |
| Ảnh biến động số dư (SMS ngân hàng) | `Báo cáo ngày/Ảnh biến động số dư/<DDMM>/` | Số dư cuối ngày từng ngân hàng + các giao dịch BVBank |

> `<DDMM>` = ngày + tháng, vd 17/06 → thư mục `1706`. (T6 = tháng 6.)
> **Số chốt quan trọng nhất:** dòng **TỒN QUỸ CÔNG TY** trên ảnh sổ quỹ — phải khớp với trường `fund` trong cả 2 file.

### B2. Các trường dữ liệu cần cập nhật

Mỗi ngày thêm **đúng 1 giá trị/ngày** cho mỗi trường. Hai file dùng **cùng một bộ số**, chỉ khác tên:

| Ý nghĩa | Trong index.html | Trong cashflow.html (`NGAY`) |
|---|---|---|
| Thu khách hàng (hoạt động) | `thu_op` | `thuKH` |
| Thu vay & tài chính | `thu_fin` | `thuVay` |
| Thu khác / hoàn quỹ | `thu_oth` | `thuKhac` |
| Chi vật tư/NCC & vận hành | `chi_op` | `chiVT` |
| Chi lãi vay & trả gốc | `chi_fin` | `chiLai` |
| Chi đầu tư (CAPEX) | `chi_cap` | `chiDT` |
| Chi khác / chi hộ | `chi_oth` | `chiKhac` |
| Tồn quỹ cuối ngày | `fund` | `fund` |
| Số dư BVBank cuối ngày | `bankEOD` | `bank` |

index.html có thêm vài trường (cashflow tự tính nên không cần): `labels` (vd `'17/06'`), `dayNum` (vd `17`), `thu` (= tổng 3 khoản thu), `chi` (= tổng 4 khoản chi), `bank` (danh sách giao dịch BVBank trong ngày), `banksEOD16` (số dư cuối kỳ TẤT CẢ ngân hàng).

### B3. Vị trí cập nhật trong cashflow.html

1. **Số liệu ngày** — mảng `const NGAY=[...]` (mở file, tìm chữ `const NGAY=`). Thêm 1 object vào **cuối** mảng cho ngày mới:

   ```
   {"k":"2026-06-17","thuKH":0,"thuVay":0,"thuKhac":0,"chiVT":0,"chiLai":0,"chiDT":0,"chiKhac":0,"fund":0,"bank":null}
   ```

   - `bank`: điền số dư BVBank cuối ngày; **chưa có ảnh SMS thì để `null`**.
   - Có ảnh số dư nhiều ngân hàng → thêm: `,"banks":[{"n":"BVBank","v":0},{"n":"VPBank","v":0},{"n":"TPBank","v":0},{"n":"Vietcombank","v":0}]`
   - → Bộ chọn ngày **tự thêm** ngày mới (nó đọc từ `NGAY`), không cần chỉnh gì khác.

2. **Ảnh chứng từ** — object `const CT={...}` (tìm `const CT=`). Thêm khóa ngày mới:

   ```
   "2026-06-17":[{"stt":1,"nd":"Nội dung giao dịch","loai":"Chi","tien":0,"imgs":[{"t":"nhãn ảnh","src":"data:image/jpeg;base64,..."}]}]
   ```

   - `loai` nhận `"Chi"`, `"Thu"` hoặc `"QT"` (quyết toán).
   - **Phần ảnh `src` (base64) bắt buộc để Claude nhúng** — xem B6.

3. **Nhãn ngày** (nếu cần) — sửa dòng chữ `📅 Chọn ngày (01–DD/06)` cho khớp khoảng ngày hiện có.

### B4. Vị trí cập nhật trong index.html

Khu vực mảng dữ liệu (tìm `const labels=`). **Mỗi mảng append đúng 1 giá trị cho ngày mới, đúng thứ tự ngày:**

- `labels` ← `'17/06'` · `dayNum` ← `17`
- `fund`, `thu`, `chi`, `thu_op`, `thu_fin`, `thu_oth`, `chi_op`, `chi_fin`, `chi_cap`, `chi_oth` ← giá trị của ngày
- `bankEOD` ← số dư BVBank cuối ngày (chưa có ảnh SMS → để `null`)
- `bank` (tìm `const bank=`): thêm từng giao dịch BVBank trong ngày theo mẫu `[17,'17/06 HH:MM','nội dung GD', SỐ_TIỀN, SỐ_DƯ_SAU_GD]` — tiền ra để **số âm**, tiền vào để **số dương**.
- `otherBanksByDay` (tìm `const otherBanksByDay=`): thêm số dư VPBank/TPBank/Vietcombank của ngày mới, khóa = số ngày (vd `17`). BVBank đã nằm trong `bankEOD` ở trên. KPI tổng, mục tổng hợp & bảng số dư 4 NH theo ngày sẽ TỰ cập nhật (`banksEOD16`/`BANKS_TOTAL16` nay tự suy ra theo ngày mới nhất, không sửa tay). Vẫn cần đổi chữ ngày hiển thị (vd 17/06) ở nhãn KPI + tiêu đề mục tổng hợp cho khớp.
- Text **"Dữ liệu 01–DD/06"** (ô góc trên + footer): cập nhật `DD` thành ngày mới nhất.

> **Quy ước:** ngày **không có ảnh SMS BVBank** → `bankEOD` = `null` (KPI sẽ hiện "(chưa có số liệu)" — đúng, **không bịa số**).

### B5. Cách kiểm tra khớp đúng giữa nguồn và website

1. **Tự kiểm trong cashflow.html:** mở file, chọn ngày → dòng ghi chú trên cùng hiện **`✓`** nếu *tổng chi từ chứng từ = tổng chi trong ngày*; nếu hiện **`⚠ lệch`** thì soát lại số/chứng từ.
2. **Khớp giữa 2 file** (mỗi ngày, các số phải bằng nhau): `thu_op`=`thuKH`, `thu_fin`=`thuVay`, `thu_oth`=`thuKhac`, `chi_op`=`chiVT`, `chi_fin`=`chiLai`, `chi_cap`=`chiDT`, `chi_oth`=`chiKhac`, `fund`=`fund`, `bankEOD`=`bank`.
3. **Khớp với sổ quỹ:** `fund` của ngày = số **TỒN QUỸ CÔNG TY** trên ảnh sổ quỹ.
4. **Khớp số dư ngân hàng:** giá trị BVBank trong `banksEOD16` = `bankEOD` ngày cuối = "số dư sau GD" của dòng cuối trong `bank` = số trên ảnh SMS BVBank.
5. **Tổng số dư:** KPI "Số dư ngân hàng" = tổng các `v` trong `banksEOD16` (tự cộng — nếu sửa 1 số mà tổng không đổi là đã gõ sai chỗ).
6. **Đếm phần tử:** mọi mảng trong index.html phải có **cùng số phần tử** = số ngày (= độ dài `labels`). Thiếu/thừa 1 giá trị sẽ làm lệch biểu đồ.

### B6. Các bước thực hiện theo trình tự (đến khi lên web)

1. Mở `Báo cáo ngày/T6/<DDMM>/` → đọc ảnh **sổ quỹ**, ghi ra 8 số: thuKH, thuVay, thuKhac, chiVT, chiLai, chiDT, chiKhac, **fund** (tồn quỹ cuối ngày). Phân loại từng phiếu vào đúng nhóm.
2. Mở `Báo cáo ngày/Ảnh biến động số dư/<DDMM>/` → ghi **số dư cuối ngày từng ngân hàng** và các **giao dịch BVBank** (giờ · nội dung · số tiền · số dư sau GD).
3. Cập nhật dữ liệu (chọn 1 trong 2 cách):
   - **Cách khuyến nghị (dễ, ít lỗi):** đưa các số ở bước 1–2 **kèm ảnh chứng từ** cho Claude và nói *"cập nhật website ngày DD/MM"*. Claude sẽ sửa đúng vị trí ở cả 2 file, **nhúng ảnh base64**, và tự kiểm khớp. (Việc nhúng ảnh base64 **bắt buộc** cần Claude.)
   - **Cách tự làm tay:** sửa cashflow.html theo **B3**, index.html theo **B4**. Phần số có thể tự gõ; riêng ảnh chứng từ vẫn cần Claude nhúng.
4. **Kiểm khớp** theo **B5** (mở thử 2 file bằng cách bấm đúp, chọn ngày mới, đối chiếu).
5. **Đẩy lên web:** làm theo mục **C** dưới đây (Commit → Push). ~1 phút sau website cập nhật.

---

## C. Đẩy lên web (Publish) — 3 bước

1. Đã sửa xong file trong `website/`.
2. Mở **GitHub Desktop** → gõ mô tả ngắn ở ô **Summary** (vd "Cập nhật ngày 17/06") → bấm **Commit to main**.
3. Bấm **Push origin** → khoảng 1 phút sau website tự cập nhật.

Một luồng duy nhất. Không upload tay, không trùng file.

---

## Lưu ý OneDrive

Thư mục `website` nằm trong OneDrive. Để tránh lỗi Git do file bị để "trên mây":
chuột phải thư mục `website` → chọn **"Luôn giữ trên thiết bị này"** (Always keep on this device). Làm 1 lần.
