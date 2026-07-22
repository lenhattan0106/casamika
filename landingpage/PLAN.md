# PLAN — Casa Mika Restaurant Landing Page (EN, Luxury)

> **Mục tiêu:** Tạo trang landing page chính cho **nhà hàng Casa Mika** (English, luxury B2C, hướng đến cả khách Việt và du khách quốc tế). Trang B2B đối tác hiện tại trở thành trang phụ.
>
> **Trạng thái:** Đang chờ confirm 3 câu hỏi ở mục **G** trước khi code.

---

## A. Cấu trúc file sau khi xong

```
landingpage/
├── index.html        ← TRANG CHÍNH MỚI (EN, luxury B2C)
├── doi-tac.html      ← TRANG B2B HIỆN TẠI (rename từ index.html cũ)
├── style.css         ← extend thêm cho trang mới (cùng theme dark+gold)
├── script.js         ← extend thêm logic mới (gallery lightbox, scroll reveal)
├── CLAUDE.md         ← cập nhật cấu trúc 2 trang
├── logo.png, image3d.jpg, ...     ← assets giữ nguyên
└── image/...
```

**Lý do giữ 1 file `style.css` + 1 `script.js`:** site nhỏ, không build step, gộp lại HTTP request ít hơn. Sẽ chia section bằng comment marker (`/* ===== MAIN SITE (index.html) ===== */`).

---

## B. Sections trang chính (EN, theo thứ tự cuộn)

| #   | Section                        | Nội dung chính                                                                                                                                                                                                                                                 | Asset dùng                                                                                  |
| --- | ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| 1   | **Hero (fullscreen)**          | Logo + tagline lớn: _"Where An Thượng Lives Slowly"_ + sub: _"A 1,400m² garden table in the heart of Da Nang"_ + 2 CTA: **Reserve a Table** / **Discover the Space**                                                                                           | `image3d.jpg` (background, subtle zoom-in Ken Burns)                                        |
| 2   | **The House** (About)          | Editorial story: concept Casa Mika, sân vườn, vị trí Phố Tây, mở 06:00–23:30. Layout 2-col: ảnh trái + text phải, gold serif heading                                                                                                                           | `image/hinh1.jpg` hoặc `image/parter.jpg`                                                   |
| 3   | **A Day at Casa Mika**         | Timeline 4 khung giờ: **Morning** (breakfast/coffee) → **Noon** (lunch) → **Evening** (dinner) → **Night** (bar & cocktail). Mỗi khung 1 ảnh + 1 dòng mood                                                                                                     | `image/3fa202a4...jpg`, `image/haisan.jpg`, `image/beeffuji.jpg`, `image/NightCocktail.jpg` |
| 4   | **Signature** (Menu highlight) | 3 card lớn hơn partner page: Asian Seafood Platter / Fuji Beef on Stone / An Thượng Night Cocktail. Mỗi card: ảnh lớn + tên + 1 dòng mô tả. Button: **View Full Menu** _(disabled cho đến khi có PDF EN — sẽ để placeholder href="#" + tooltip "Coming soon")_ | reuse haisan/beeffuji/NightCocktail                                                         |
| 5   | **Gallery**                    | Grid 6 ảnh kiểu masonry (3 lớn + 3 nhỏ), click mở lightbox phóng to. Tận dụng các ảnh chưa dùng                                                                                                                                                                | brochure.jpg, deal.jpg, avatar.jpg, image3d.jpg, hinh1.jpg, parter.jpg                      |
| 6   | **Reserve Your Table**         | Layout 2-col: trái = nội dung "Open daily 06:00 – 23:30, advance booking recommended"; phải = 3 nút lớn: 📞 **Call +84 708 888 007** · 💬 **Chat on Zalo** · 📍 **Get Directions**. Embed Google Maps iframe phía dưới.                                        | (không cần)                                                                                 |
| 7   | **Footer**                     | Logo + address + hours + social (FB + IG icon) + nhỏ ở góc phải: _"For tour operators & partners → Vietnamese B2B page"_ link sang `doi-tac.html`                                                                                                              | logo.png                                                                                    |

**Bỏ:** form đặt bàn (không cần backend). **Bỏ:** testimonials/reviews (chưa có review thật). Có thể thêm sau.

---

## C. Design system (extend từ style.css hiện tại)

**Giữ nguyên:**

- Gold `#B8912A`, dark `#1A1410`, cream `#FAF7F2`
- Playfair Display (heading) + Inter (body)
- `.fade-up`, `.fade-in` animation đã có

**Thêm mới:**

- **Hero parallax / Ken Burns zoom** — CSS animation `transform: scale(1) → scale(1.08)` 20s loop
- **Editorial 2-column layout** — `.section-editorial` cho About + Reserve sections
- **Gallery masonry** — CSS Grid với `grid-auto-rows` + click-to-zoom lightbox (JS)
- **Timeline section** — 4 cards ngang với divider dọc gold giữa mỗi cột (responsive: stack mobile)
- **Larger typography** — Hero title 5rem desktop / 3rem mobile (lớn hơn partner page)
- **Sticky reserve CTA** — nút "Reserve" nhỏ luôn hiện góc phải dưới khi cuộn (mobile-friendly)

---

## D. Navigation 2 chiều giữa 2 trang

- **`index.html` (main):** footer có dòng nhỏ `"For tour operators & partners → Vietnamese B2B page"` → link `doi-tac.html`
- **`doi-tac.html` (partner):** thêm 1 dòng nhỏ ở navbar trên cùng: `"← Trở về trang chính"` → link `index.html`

---

## E. Thông tin liên hệ (đã chốt, sẽ hard-code vào HTML)

```
Hotline:   0708 888 007       → tel:+84708888007
Zalo:      0708 888 007       → https://zalo.me/0708888007
Email:     info@casamika.com  → mailto:info@casamika.com
Facebook:  https://www.facebook.com/official.casamika
Instagram: https://www.instagram.com/casamika.official/
Maps:      https://maps.app.goo.gl/qsTUVr82UATkvFJv5
Address:   37 Trần Bạch Đằng, An Thượng, Ngũ Hành Sơn, Da Nang
Hours:     Daily, 06:00 – 23:30
```

(Cũng sẽ cập nhật luôn các placeholder `https://zalo.me/` trong `doi-tac.html` cho đồng nhất.)

---

## F. Implementation tasks (theo thứ tự thực hiện)

1. **Rename** `index.html` → `doi-tac.html` (dùng `git mv` để giữ history)
2. **Update** `doi-tac.html`: thêm nav link "← Trở về trang chính" + thay thông tin Zalo/hotline thực
3. **Tạo mới** `index.html` (EN, 7 sections trên)
4. **Extend** `style.css`: thêm phần CSS cho trang mới (đánh dấu bằng comment block)
5. **Extend** `script.js`: thêm gallery lightbox + sticky CTA + Ken Burns observer
6. **Embed** Google Maps iframe trong Reserve section
7. **Cập nhật** `CLAUDE.md`: cấu trúc 2 trang, navigation, asset mapping
8. **Test** local: `python3 -m http.server 8000` — kiểm cả 2 trang, mobile responsive (DevTools)

**Ước lượng:** ~600–800 dòng HTML mới + ~300–400 dòng CSS mới + ~80 dòng JS mới.

---

## G. 3 thứ cần confirm trước khi code

1. **Tagline Hero:** đề xuất _"Where An Thượng Lives Slowly"_ — anh thích không, hay muốn tagline khác?
   - Phương án khác: _"The Garden Table of Da Nang"_ / _"An Thượng, Reimagined"_ / _"A Sanctuary in the Old Quarter"_

2. **"View Full Menu" button:** chưa có menu PDF tiếng Anh.
   - Phương án A: để **disabled với label "Coming Soon"**
   - Phương án B: **ẩn luôn** button cho đến khi có PDF

3. **Sticky Reserve CTA góc phải dưới:** anh OK với UX này không?
   - Hữu ích trên mobile nhưng có người thấy phiền
   - Có thể chỉ hiện sau khi cuộn qua Hero, và ẩn ở section Reserve

---

## H. Decision log (cập nhật khi có thay đổi)

| Date       | Decision                                                                                                                                                                                                                                                            | Note                    |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| 2026-06-10 | Routing: `index.html` (main, EN) + `doi-tac.html` (partner, VN)                                                                                                                                                                                                     | Confirmed               |
| 2026-06-10 | CTA chính: Zalo + Hotline + Google Maps (không form đặt bàn)                                                                                                                                                                                                        | Confirmed               |
| 2026-06-10 | Ngôn ngữ: English only                                                                                                                                                                                                                                              | Confirmed               |
| 2026-06-10 | Theme: giữ dark + gold (đồng nhất brand)                                                                                                                                                                                                                            | Confirmed               |
| 2026-06-10 | Assets: dùng ảnh có sẵn, sau thay ảnh thật                                                                                                                                                                                                                          | Confirmed               |
| 2026-06-10 | Thông tin liên hệ: hotline/zalo/maps/social đã chốt                                                                                                                                                                                                                 | Confirmed               |
| 2026-06-10 | G1 — Tagline "Where An Thượng Lives Slowly"                                                                                                                                                                                                                         | Confirmed               |
| 2026-06-10 | G2 — Menu button: disabled + "Coming Soon"                                                                                                                                                                                                                          | Confirmed (Phương án A) |
| 2026-06-10 | G3 — Sticky Reserve CTA: giữ, hiện sau hero, ẩn ở Reserve section                                                                                                                                                                                                   | Confirmed               |
| 2026-06-10 | **iPhone responsive là yêu cầu bắt buộc** — mobile-first CSS, breakpoints 640/768/1024/1280, touch target ≥48px, safe-area-inset cho notch, `100svh` cho hero, hamburger nav, lightbox touch-swipe, `-webkit-tap-highlight-color`, `prefers-reduced-motion` respect | Confirmed               |
