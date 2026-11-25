# BTTH01 — Phân tích Website Trường Đại học Thủy lợi (`https://tlu.edu.vn/`) theo 3 lớp song song

- **Sinh viên thực hiện:** Từ Thị Trà My — Lớp Hệ thống thông tin (Năm 2)
- **Website phân tích:** `https://tlu.edu.vn/`
- **Phương pháp:** Phân tích từ ngoài vào trong, từ trên xuống dưới theo 3 lớp song song: **Layout Box — CSS Layout Technique — Content & Semantic HTML**.

> **Phân biệt dữ liệu quan sát và suy luận:**
> - **Quan sát trực tiếp:** Cấu trúc các vùng giao diện, menu, danh mục tin tức, thông báo, sự kiện, số liệu, footer và bản đồ được đối chiếu trực tiếp từ ảnh chụp màn hình thực tế trong thư mục `images/`.
> - **Suy luận kỹ thuật:** Các thuộc tính CSS đề xuất (Flexbox, Grid, Position) và cấu trúc HTML Semantic trong `index.html`, `style.css` là giải pháp chuẩn hóa phục vụ học tập, không sao chép nguyên văn mã nguồn thực tế (Next.js/Tailwind) của hệ thống sản xuất.

---

## Ảnh chụp thực tế & Sơ đồ bố cục

### Sơ đồ khoanh hộp 3 lớp toàn trang
![Sơ đồ bố cục 3 lớp](images/layout-diagram.svg)

### Ảnh chụp chi tiết Header (Desktop + Dropdown) & Footer (Desktop 4 cột)
| Header & Hero Banner (Desktop) | Footer 4 cột & Google Maps (Desktop) |
| :---: | :---: |
| ![Header Desktop](images/header-desktop.png) | ![Footer Desktop](images/footer-desktop.png) |

*(Ảnh chụp toàn trang chủ được lưu tại [`images/homepage-full.png`](images/homepage-full.png))*

---

## BƯỚC 1 — XÁC ĐỊNH CẤU TRÚC TỔNG THỂ

```text
page
├── site-header (Logo + Ô tìm kiếm + Chọn ngôn ngữ VN/EN)
├── main-nav (Thanh điều hướng chính 8 chuyên mục + Dropdown)
├── hero-banner (Ảnh bìa sự kiện 20/11 + Nút điều hướng slide)
├── floating-social (Thanh icon mạng xã hội cố định bên phải)
├── main-content
│   ├── news-notice-section (Tin tức & Thông báo)
│   ├── events-section (Sự kiện)
│   ├── stats-section (Những con số ấn tượng)
│   ├── admission-section (Tuyển sinh)
│   ├── academic-student-section (Đào tạo / Sinh viên / Cựu sinh viên)
│   ├── units-section (Các đơn vị đào tạo)
│   ├── research-coop-section (Hợp tác quốc tế & Nghiên cứu)
│   ├── partners-community-section (Mạng lưới đối tác & Vì cộng đồng)
│   └── gallery-section (Khoảnh khắc TLU)
└── site-footer (Thông tin liên hệ + 4 cột liên kết/bản đồ + Bản quyền)
```

| Vùng lớn | Tên layout đề xuất | Vai trò | Phạm vi giao diện | Quan hệ với vùng cha | Cần `.container`? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Header** | `site-header` | Nhận diện thương hiệu, tìm kiếm, đổi ngôn ngữ | Trên cùng, rộng 100% | Con trực tiếp của `page` | **Có** |
| **Navigation** | `main-nav` | Điều hướng 8 chuyên mục chính | Ngay dưới Header, rộng 100% | Con trực tiếp của `page` | **Có** |
| **Hero** | `hero-banner` | Hiển thị banner sự kiện nổi bật (20/11) | Ngay dưới Nav, rộng 100% | Con trực tiếp của `page` | **Không** (tràn viền) |
| **Main** | `main-content` | Chứa toàn bộ các khối nội dung chính | Phần thân giữa trang | Con trực tiếp của `page` | **Có** (theo từng `section`) |
| **Footer** | `site-footer` | Địa chỉ 175 Tây Sơn, liên kết, bản đồ, bản quyền | Dưới cùng, rộng 100% | Con trực tiếp của `page` | **Có** |

---

## BƯỚC 2 — PHÂN TÍCH TỪNG COMPONENT THEO 3 LỚP SONG SONG

### 1. SITE HEADER (`site-header`)
- **Layout Box:** `site-header > container > header-inner (brand + header-tools)`
- **CSS Layout:** Flexbox (`display: flex; justify-content: space-between; align-items: center; gap: 20px;`).
- **Nội dung & HTML:** Logo (`a > img`), Ô tìm kiếm (`form > input + button`), Ngôn ngữ (`a`).
- **Chỉ dùng `div`:** `<div class="site-header"><div class="container header-inner"><div class="brand">...</div><div class="header-tools">...</div></div></div>`
- **HTML Semantic:** `<header class="site-header">...<a class="brand"><img ...></a><form class="search-form">...</form></header>`
- **Kết luận tư duy:** `Layout: brand + tools` → `CSS: Flexbox row (space-between)` → `HTML: header, a, img, form, input, button`.

### 2. MAIN NAVIGATION (`main-nav`)
- **Layout Box:** `main-nav > container > nav-list > nav-item[]` (kèm `dropdown-menu` bên trong các mục có mũi tên xuống như *Giới thiệu, Tin tức...*).
- **CSS Layout:** Flexbox (`display: flex; justify-content: space-between; align-items: center;`) kết hợp `position: relative` ở `.nav-item` và `position: absolute` ở `.dropdown-menu`.
- **Nội dung & HTML:** Điều hướng (`nav`), danh sách (`ul > li > a`).
- **Kết luận tư duy:** `Layout: nav-list → nav-item` → `CSS: Flexbox row + Position dropdown` → `HTML: nav, ul, li, a`.

### 3. HERO BANNER & FLOATING SOCIAL (`hero-banner`, `floating-social`)
- **Layout Box:** `hero-banner (hero-slide + hero-dots)` và `floating-social` (cụm icon Facebook, YouTube, Instagram, TikTok).
- **CSS Layout:** `hero-banner` dùng Block Flow + `position: relative/absolute` cho chấm chuyển slide. `floating-social` dùng `position: fixed; right: 16px; top: 30%; display: flex; flex-direction: column;`. *(Ghi chú: Hero có khả năng là Slider/Carousel tự động chuyển slide, cần kiểm tra JS/DOM để xác nhận).*
- **Nội dung & HTML:** `section`, `img`, `button` (nút chấm slide), `aside` / `nav` (mạng xã hội).

### 4. TIN TỨC & THÔNG BÁO (`news-notice-section`)
- **Layout Box:** `news-notice-layout` gồm 2 cột: `news-column` (trái) và `notice-column` (phải).
- **CSS Layout:** Flexbox row với tỷ lệ **2 : 1** (`.news-column { flex: 2; }`, `.notice-column { flex: 1; }`). Bên trong từng `news-card` dùng Flexbox row (`thumb` cố định + `body` `flex: 1`), bên trong `news-body` dùng **Normal Flow**.
- **Nội dung & HTML:** `section`, `h2`, `article` (từng tin), `img`, `h3 > a`, `p`, `time`.

### 5. SỰ KIỆN & NHỮNG CON SỐ ẤN TƯỢNG (`events-section`, `stats-section`)
- **Layout Box:** `events-grid` (các thẻ sự kiện) và `stats-grid` (5 chỉ số: Người học, Đối tác, Việc làm, Ngành học, Giảng viên Tiến sĩ).
- **CSS Layout:** CSS Grid (`grid-template-columns: repeat(5, 1fr)` cho khối số liệu; `repeat(2, 1fr)` hoặc Flex Carousel cho Sự kiện).
- **Nội dung & HTML:** `section`, `h2`, `article`, `dl / dt / dd` (cặp số liệu – nhãn).

### 6. TUYỂN SINH & KHỐI 3 CỘT ĐÀO TẠO / SINH VIÊN / CỰU SINH VIÊN
- **Layout Box:** `admission-box` (Flexbox 2 cột 1 : 1: ảnh trái + nội dung phải) và `triple-grid` (3 cột đều nhau).
- **CSS Layout:** Flexbox `1 : 1` cho Tuyển sinh; **CSS Grid** `repeat(3, 1fr)` cho Đào tạo – Sinh viên – Cựu sinh viên.
- **Nội dung & HTML:** `section`, `h2`, `article`, `img`, `h3`, `p`, `a`.

### 7. KHOẢNH KHẮC TLU (`gallery-section`)
- **Layout Box:** `gallery-grid` chứa lưới ảnh đa kích thước (dạng Bento/Grid).
- **CSS Layout:** CSS Grid (`grid-template-columns: repeat(4, 1fr); gap: 16px;`).
- **Nội dung & HTML:** `section`, `h2`, `figure > img`.

### 8. SITE FOOTER (`site-footer`)
- **Layout Box:** 3 tầng dọc: `footer-top` (Logo + Địa chỉ 175 Tây Sơn + Email + Điện thoại) → `footer-columns` (4 cột: Giới thiệu, Truy cập nhanh, Theo dõi TLU, Bản đồ Google Maps) → `footer-bottom` (Bản quyền).
- **CSS Layout:** `footer-top` dùng Flexbox row; `footer-columns` dùng **CSS Grid 4 cột** (`repeat(4, 1fr)`); `footer-bottom` dùng Normal Flow (`text-align: center`).
- **Nội dung & HTML:** `footer`, `address`, `h3`, `ul > li > a`, `iframe` (Google Maps), `p > small`.

---

## BƯỚC 3 — BẢN ĐỒ FLEXBOX TRÊN TOÀN TRANG

```text
PAGE
├── HEADER: header-inner [FLEX row] → header-tools [NESTED FLEX row]
├── NAV: nav-list [FLEX row]
├── HERO: [KHÔNG CẦN FLEX - Block + Position]
├── MAIN
│   ├── news-notice-layout [FLEX row, tỷ lệ 2 : 1]
│   │   └── news-card [NESTED FLEX row]
│   ├── admission-box [FLEX row, tỷ lệ 1 : 1]
│   └── stats-grid / triple-grid / gallery-grid [DÙNG GRID]
└── FOOTER
    ├── footer-top [FLEX row, space-between]
    └── footer-columns [DÙNG GRID 4 cột]
```

---

## BƯỚC 4 — NHẬN DIỆN CSS GRID

CSS Grid phù hợp hơn Flexbox tại các vùng có cấu trúc hàng × cột đều nhau:
- **Những con số ấn tượng:** `grid-template-columns: repeat(5, 1fr);`
- **Đào tạo / Sinh viên / Cựu sinh viên:** `grid-template-columns: repeat(3, 1fr);`
- **Footer 4 cột:** `grid-template-columns: repeat(4, 1fr);`
- **Khoảnh khắc TLU:** `grid-template-columns: repeat(4, 1fr);`

---

## BƯỚC 5 — HÀNH VI RESPONSIVE

- **Desktop (`> 1024px`):** Menu ngang 8 mục; Tin tức/Thông báo chia 2 cột `2 : 1`; Footer 4 cột.
- **Tablet (`768px – 1024px`):** Menu thu gọn thành nút 3 gạch (`☰`); Footer co từ 4 cột xuống **2 cột**; Lưới 3 cột chuyển thành 2 cột.
- **Mobile (`< 768px`):** Toàn bộ cột Flexbox chuyển sang `flex-direction: column`; toàn bộ CSS Grid chuyển về `1fr` xếp dọc.

---

## BƯỚC 6 — CÂY DOM GIẢ LẬP TOÀN TRANG

```text
body
├── header.site-header > div.container.header-inner (a.brand, div.header-tools)
├── nav.main-nav > div.container > ul.nav-list > li.nav-item > a
├── aside.floating-social > ul > li > a
├── main.main-content
│   ├── section.hero-banner
│   ├── section.news-notice-section > div.news-notice-layout (article.news-card, ul.notice-list)
│   ├── section.stats-section > div.stats-grid > dl.stat-item
│   ├── section.admission-section > div.admission-box
│   ├── section.triple-section > div.triple-grid > article.info-card
│   └── section.gallery-section > div.gallery-grid > figure
└── footer.site-footer > div.container (div.footer-top, div.footer-columns, div.footer-bottom)
```

---

## BƯỚC 7 — BẢNG TỔNG HỢP TOÀN TRANG

| Component | Layout Box | Layout Technique | Flex Container? | Flex Items | Nội dung | HTML Semantic |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Header | `header-inner` | Flexbox | Có | `brand`, `header-tools` | Logo, tìm kiếm | `header`, `a`, `img`, `form`, `input`, `button` |
| Navigation | `nav-list` | Flexbox + Position | Có | `nav-item` | Menu 8 mục + dropdown | `nav`, `ul`, `li`, `a` |
| Hero Banner | `hero-banner` | Block + Position | Không | - | Ảnh 20/11, nút slide | `section`, `img`, `button` |
| Tin tức & Thông báo | `news-notice-layout` | Flexbox (2 : 1) | Có | `news-column`, `notice-column` | Bài viết, thông báo | `section`, `h2`, `article`, `time`, `p` |
| Số liệu ấn tượng | `stats-grid` | CSS Grid (5 cột) | Không | - | Số liệu thống kê | `section`, `dl`, `dt`, `dd` |
| Đào tạo / SV / CSV | `triple-grid` | CSS Grid (3 cột) | Không | - | Thẻ chuyên mục | `section`, `article`, `img`, `h3`, `p` |
| Footer | `footer-columns` | CSS Grid (4 cột) | Không | - | Địa chỉ, link, bản đồ | `footer`, `address`, `ul`, `li`, `a`, `small` |

---

## BƯỚC 8, 9, 10 — CHECKLIST TƯ DUY, ĐẶT TÊN CLASS & LỖI CẦN TRÁNH

- **Checklist 8 bước:** Nhìn giao diện → Khoanh hộp → Đặt tên theo vai trò → Xác định cha–con → 1 trục ngang dùng Flexbox → Lưới hàng×cột dùng Grid → Xếp dọc giữ Normal Flow → Chọn HTML Semantic.
- **Quy tắc đặt tên:** Dùng tên theo vai trò (`site-header`, `nav-list`, `news-card`, `footer-columns`), tránh `box1`, `blue-box`, `left-div`.
- **Lỗi cần tránh:** Lạm dụng Flexbox cho thẻ xếp dọc, nhầm lẫn `<a>` và `<button>`, lạm dụng thẻ `<div>` ("div-itis"), quên `.container` căn giữa.
