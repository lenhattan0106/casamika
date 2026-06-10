# Casa Mika — Landing Pages

Site có **2 trang HTML** chạy chung style.css + script.js:

| URL | File | Audience | Ngôn ngữ | Mục đích |
|---|---|---|---|---|
| `/` | **`index.html`** | Khách lẻ (chính) | **English** | Luxury B2C, đặt bàn |
| `/doi-tac.html` | **`doi-tac.html`** | Đối tác (HDV, tài xế, tour) | Tiếng Việt | B2B partnership (commission) |

Stack: HTML/CSS/JS thuần, **không build step**. Deploy Vercel (push main branch → auto).

---

## Cấu trúc thư mục

```
landingpage/
├── index.html        # TRANG CHÍNH — EN luxury B2C
├── doi-tac.html      # TRANG B2B — VN partner (có back-link sang index)
├── style.css         # 1 file chung — phần MAIN SITE đánh dấu bằng banner comment
├── script.js         # 1 file chung — đều dùng null-check để chạy được trên 2 trang
├── PLAN.md           # Plan ban đầu của trang mới (đã hoàn thành)
├── CLAUDE.md         # File này
├── requirement.txt   # Python deps (pandas, openpyxl) + ghi chú vận hành
├── mika_analysis.py  # Sinh restaurant_cost_analysis_mika_casa.xlsx
├── restaurant_cost_analysis_mika_casa.xlsx
├── Casa Mika Portfolio tiếng việt.pdf   # link "Tải Hồ Sơ" trên doi-tac.html
├── logo.png, image3d.jpg               # Hero background + CTA bg cho cả 2 trang
├── avatar.jpg, brochure.jpg, deal.jpg  # Gallery trang chính
├── Màu be và Nâu Sẫm...Email.jpg
└── image/
    ├── hinh1.jpg          # Policy (B2B) + About (main) + Gallery (main)
    ├── haisan.jpg         # Signature seafood (cả 2 trang)
    ├── beeffuji.jpg       # Signature beef (cả 2 trang)
    ├── NightCocktail.jpg  # Signature cocktail (cả 2 trang)
    ├── parter.jpg         # Gallery trang chính
    └── 3fa202a4...jpg     # Morning shot trang chính
```

---

## index.html (trang chính, EN)

**Tagline:** *"Where An Thượng Lives Slowly"*

### Sections (theo thứ tự cuộn)
1. **Nav** (`#msNav`) — fixed, blur background khi cuộn (>30px). Mobile = hamburger drawer.
2. **Hero** (`#hero`) — fullscreen `100svh`, `image3d.jpg` background + Ken Burns zoom 22s, eyebrow address, H1 italic gold "Lives Slowly", 2 CTA: **Reserve a Table** / **Discover the Space**.
3. **The House** (`#house`) — Editorial 2-col: ảnh `image/hinh1.jpg` + text + 3 stat metas (1,400m² · 06:00–23:30 · Asian·Western).
4. **A Day at Casa Mika** (`#day`) — 4-card timeline ngang: **Morning** / **Noon** / **Evening** / **Night**, mỗi card có time + name + mood line.
5. **Signature** (`#signature`) — 3 card lớn: Asian Seafood Platter / Fuji Beef on Stone / An Thượng Night. Button **"View Full Menu — Coming Soon"** đang `disabled` (chờ menu PDF tiếng Anh).
6. **Gallery** (`#gallery`) — Masonry grid 6 ảnh (`ms-gal-tall` span 2 rows; `ms-gal-wide` span 2 cols ở desktop). Click → lightbox với keyboard (←/→/Esc) + swipe (touch).
7. **Reserve** (`#reserve`) — 2-col: text + 3 nút contact card (Call / Zalo / Maps) + Google Maps iframe embed.
8. **Footer** — Logo + Visit + Contact + Follow (FB/IG SVG icons). Bottom có dòng nhỏ link sang `doi-tac.html` cho partner.

### UI patterns đặc biệt
- **Sticky Reserve CTA** (`#msStickyCta`) — fab góc phải dưới, hiện khi cuộn qua ~85% hero, **ẩn khi đang ở Reserve section** (tránh dư thừa).
- **Mobile hamburger nav** (`#msNavToggle`) — toggle drawer fullscreen, đóng khi click link / Esc.
- **Lightbox** (`#msLightbox`) — swipe ngang prev/next, swipe xuống đóng, click ngoài đóng.

---

## doi-tac.html (trang B2B, VN)

- Có **`.back-to-main`** bar trên cùng: "← Trở về trang chính Casa Mika" → `index.html`.
- Logo trong navbar giờ link về `index.html` (trước là `#`).
- Link Zalo placeholder cũ `https://zalo.me/` đã thay thành `https://zalo.me/0706024684`.
- Footer thêm dòng hotline + Zalo.
- Còn lại nguyên bản: Hero (image3d.jpg), Giá Trị Cốt Lõi, Chính Sách, Ẩm Thực, Bảng Giá (Standard/Premium/VIP), CTA, Modal menu Unsplash.

---

## Design system (1 file `style.css`)

File chia 2 phần qua banner comment lớn:
- **Phần trên** (~lines 1–1699): tất cả CSS cho `doi-tac.html` (giữ nguyên không sửa).
- **Phần dưới** (sau banner `MAIN SITE — index.html`): CSS cho trang chính, prefix `.ms-*` + scope `.site-main`.

**Token chung (dùng cho cả 2):**
- Gold `#B8912A` · Gold light `#D4A94A` · Dark `#1A1410` · Cream `#FAF7F2`
- Heading: `'Playfair Display'`, italic em = gold
- Body: `'Inter'`
- Animation classes: `.fade-up`, `.fade-in`, `.delay-1`, `.delay-2`, `.delay-3`

### Mobile-first / iPhone optimization (CHỈ trang chính)
- **`100svh`** cho hero — fix iOS Safari address-bar height issue
- **`env(safe-area-inset-top/bottom)`** — nav, sticky CTA, footer bottom, lightbox close button (notch + home indicator)
- **`-webkit-tap-highlight-color: transparent`** — bỏ flash xám khi tap iOS
- **`min-height: 48px`** trên mọi button/contact card/lightbox button — Apple HIG touch target
- **`clamp(min, vw, max)`** cho typography — fluid scale, không cần n media queries
- **Breakpoints:** 480 / 640 / 768 / 1024 / 1280
- **Touch swipe** trong lightbox: ngang = prev/next, xuống = đóng
- **`@media (prefers-reduced-motion: reduce)`** — tắt Ken Burns + scroll pulse + hover scale
- **Backdrop-filter** có cả `-webkit-` prefix (Safari)

---

## script.js — 1 file dùng cho 2 trang

Tất cả init bằng `DOMContentLoaded`. Mỗi block đều **null-check element trước** để không vỡ khi chạy trên trang còn lại.

- **Old (doi-tac):** sticky navbar (`#navbar`), smooth scroll anchor, IntersectionObserver fade, modal menu (`#menuModal`).
- **Main site (index):**
  - `#msNav` scrolled class + `#msStickyCta` show/hide logic
  - `#msNavToggle` hamburger drawer + aria-expanded
  - `#msLightbox` click/keyboard/swipe handlers
- IntersectionObserver fade dùng chung cho cả 2 trang (cả `.fade-up`, `.fade-in` đều bắt được).

---

## Thông tin liên hệ (chốt, hard-code 2 trang)

```
Hotline:   0708 888 007       → tel:+84708888007
Zalo:      0706 024 684       → https://zalo.me/0706024684
Email:     info@casamika.com  → mailto:
Facebook:  https://www.facebook.com/official.casamika
Instagram: https://www.instagram.com/casamika.official/
Maps:      https://maps.app.goo.gl/qsTUVr82UATkvFJv5
Address:   37 Trần Bạch Đằng, An Thượng, Ngũ Hành Sơn, Da Nang
Hours:     Daily, 06:00 – 23:30
```

Google Maps embed trong `index.html` dùng URL `https://maps.google.com/maps?q=37%20Tr%E1%BA%A7n%20B%E1%BA%A1ch%20%C4%90%E1%BA%B1ng%2C%20An%20Th%C6%B0%E1%BB%A3ng%2C%20Da%20Nang&output=embed` (search-by-address embed) vì share link rút gọn `maps.app.goo.gl/...` không thể nhúng iframe trực tiếp.

---

## Vận hành

```bash
# Chạy local
python3 -m http.server 8000
# → http://localhost:8000              (trang chính EN)
# → http://localhost:8000/doi-tac.html  (trang B2B VN)

# Sinh Excel phân tích chi phí
pip install -r requirement.txt
python3 mika_analysis.py

# Deploy (Vercel push-to-main)
git add .
git commit -m "..."
git push origin main
```

---

## Việc cần làm sau (TODO)

- [ ] **Menu PDF tiếng Anh** → uncomment / enable nút `#msMenuBtn` trong index.html khi có file
- [ ] **Ảnh thật** → thay 6 ảnh gallery + 4 ảnh "A Day at Casa Mika" + ảnh About bằng food photography xịn
- [ ] **Google Maps embed pin chính xác** → có thể replace bằng pb-URL từ Google Maps Share → Embed (lấy iframe gốc) cho pin Casa Mika chuẩn hơn search-by-address
- [ ] **Brand logo** — nếu có version logo nền tối cleanup hơn → thay `logo.png`

---

## Lưu ý cho session sau

- **Đường dẫn:** tất cả relative trong `landingpage/`, không có `../`. Không cần sửa khi move file.
- **2 trang dùng chung `style.css` + `script.js`** — đừng tách. Phần MAIN SITE CSS được đánh dấu rõ bằng banner comment giữa file.
- **Repo git root** ở `Casa Mika/` (thư mục cha), không phải `landingpage/`. Sibling `cost-management-casamika/` là project khác (web app FastAPI + React).
- **PLAN.md** là plan ban đầu — giờ chỉ giữ làm decision log lịch sử. Tham chiếu thực tế đọc CLAUDE.md (file này).
