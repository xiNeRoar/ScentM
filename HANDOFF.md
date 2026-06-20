# Scent.M — Developer Handover

純靜態 HTML/CSS/JS 設計交付物。下一步係喺 WordPress 落地。本文件涵蓋 tech stack、頁面結構、SEO 待辦、效能待辦。

---

## 1. Tech Stack(目標實作)

| 層 | 用咩 |
|---|---|
| CMS | WordPress(純內容,無 page builder) |
| Base theme | Blocksy + Custom Child Theme |
| 版面 | 手寫 Tailwind HTML/PHP 寫入 template |
| 自訂欄位 / 模組 | **ACF Pro**(Flexible Content 做可重排 section) |
| CPT | **CPT UI** — `workshops`、`journal` |
| 預約 + 收費 | **SureCart**(無 WooCommerce)— workshop = product,inventory = 名額,slide-out checkout,Stripe(Visa/Master/Apple Pay/Google Pay/AlipayHK/WeChat Pay HK)+ FPS offline |
| 表單 | **Forminator** 或 **Fluent Forms**(Raw HTML 輸出,完全自訂 CSS) |
| Newsletter | Mailchimp / Klaviyo + double opt-in → `newsletter-confirm.html` |
| 地圖 | **Leaflet.js + OpenStreetMap/CartoDB**(已運作,無需 API key) |
| 動畫 | GSAP 3.12.5 + ScrollTrigger;Lenis smooth scroll |
| 字體 | Google Fonts:Baskervville、Cactus Classical Serif、Mea Culpa、Montserrat、Noto Sans TC |
| 圖示 | Font Awesome |
| SEO | Yoast / RankMath |
| Cookie consent | Cookiebot / Iubenda(PDPO/GDPR,因有 GA) |

**唔用:** WooCommerce、Elementor/Divi、Google Maps API。

---

## 2. 頁面清單

| 檔案 | 用途 | WP 實作 |
|---|---|---|
| index.html | 首頁 | 自訂 front-page template |
| about.html | 品牌故事 | static template |
| odm-perfume / -home / -personal / -pet.html | ODM 四大分類(獨立 URL + tab soft-nav) | static templates 或 CPT |
| workshops.html | 工作坊列表 | `workshops` CPT archive |
| workshop-detail.html | 單一工作坊 | `workshops` CPT single + SureCart |
| journal.html | 日誌列表 | `journal` CPT archive |
| journal-detail.html | 單一文章 | `journal` CPT single + ACF Flexible |
| testimonials.html | 客戶見證 | ACF repeater |
| contact.html | 聯絡 + 表單 | Forminator + Leaflet |
| thanks.html / newsletter-confirm.html | 提交後 landing | static(noindex) |
| 404.html | 404 | theme 404 |
| privacy.html / terms.html | 法律 | static |

---

## 3. ODM Tab Soft-Navigation(重要)

4 個 ODM 頁係**獨立真 HTML/URL**(SEO + 可直接 crawl)。前端用 **AJAX partial swap + history.pushState** 做切換:
- 禁 tab → `fetch` 目標頁 → 換走 `#odm-swap` 內容 → 更新 URL/title
- **Tab nav(`#odm-tabs`)喺 `#odm-swap` 之外,係持久元素**(唔重建,避免 sticky flash);active 狀態由 `syncTabs()` 同步
- swap 後 `revealNative()` 即時顯示新內容、`reExec()` 重跑頁面 script
- 失敗自動 fallback 正常 navigation;有 popstate 支援
- **WP 實作注意:** 確保每個 ODM 頁 server-render 完整(SEO),前端 JS 只做漸進增強

---

## 4. ACF Flexible Content blocks(已喺 HTML demo)

**Workshop Detail / Journal Detail 共用:**
`rich_text`(可選首段 drop cap)、`section_heading`、`single_image`、`photo_album`(3-col)、`video_embed`、`pull_quote`;Workshop 另加 `session_timeline`、`whats_included`、`studio_map`(Leaflet)。

每個 block 可自由新增 / 刪除 / 重排。

---

## 5. 設計語言(以 index/about/workshops/journal 為準)

- 色:light `#FDFBF9` / beige `#F3EAE1` / terracotta `#A64B2A` / brown `#3B261D`
- 字:serif Baskervville+Cactus / sans Montserrat+Noto Sans TC / script Mea Culpa(極少用)
- 雙語標題:中文 + `<span italic terracotta>English</span>`
- Fluid type:`clamp()` 768→1600px,1600+ plateau
- 動畫:`.ani-this bottom-in` → CSS transition 0.8s `cubic-bezier(0.25,1,0.35,1)` + `delay-N` stagger;由 IntersectionObserver 加 `.in`
- Flip / morph 特效**只限**首頁 hero CTA + nav;其他頁禁用
- CTA 用字統一:詢問 =「聯絡我們」、booking =「立即預約」、外部 = Explore Hibi Lab
- 浮動 WhatsApp = 主要聯絡渠道,**唔好再加重複 contact CTA**

---

## 6. SEO — 已完成(靜態階段)

- 每頁:獨立 title + meta description(中文)+ canonical + OG + Twitter Card
- `lang="zh-HK"`、語義 heading、圖片 alt
- `sitemap.xml`、`robots.txt`
- Structured data:Organization + LocalBusiness(index/contact)、Service(4 ODM)、Event(workshop-detail)、Article(journal-detail)
- `thanks` / `404` / `newsletter-confirm` 已 `noindex`

### ⚠️ 部署前必改
- 全站 placeholder 域名 **`https://www.scentm.hk`**(在 canonical / og:url / sitemap.xml / robots.txt / JSON-LD)→ 換正式域名
- `sitemap.xml` 的 `lastmod` 寫死 → 接 CMS 動態

---

## 7. SEO / 效能 — 待 WordPress 階段做(靜態做唔到)

1. **Tailwind CDN → build purged 靜態 CSS**(目前用 `cdn.tailwindcss.com`,生產唔可用)
2. 字體 `preload` + `font-display: swap`
3. 圖片 **WebP + `srcset` + `width`/`height`**(減 CLS / LCP,Core Web Vitals)
4. JS `defer` / `preconnect` / 移除未用 CDN
5. 真實 **1200×630 OG 分享圖**(目前用 hero.jpg)
6. 接 **Google Search Console + Analytics**、提交 sitemap
7. SSL、301、www↔non-www 統一
8. Google Business Profile(本地 SEO)、持續 journal 內容

---

## 8. 第三方資源(CDN,已寫入 HTML)
- GSAP 3.12.5 + ScrollTrigger、Lenis 1.1.14、Leaflet 1.9.4、Font Awesome 6.5.2、Google Fonts、Tailwind(CDN — 生產要 build)

---

## 9. 圖片資產
`hero.jpg`(首頁/about/journal hero)、`odm-perfume.jpg` + `perfume-1~4.jpg`(ODM 香水)、`marketing.jpg`、`journal-1~3.jpg`、`scentm-logo.png`、`hibi-lab-logo.png`(CSS mask,跟 theme 變色)。全部相對路徑(flat,同 repo 根目錄)。

_交付:設計階段完成。下一步 = WordPress 落地 + 效能優化 + 域名 / 分析接入。_
