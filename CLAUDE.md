# CLAUDE.md — Cycling for Charity Japan Website

This file gives Claude Code instant context about this project. Read it before making any changes.

## Project Overview

**Organisation:** NPO法人 サイクリング・フォー・チャリティー (Cycling for Charity Japan)  
**Website:** https://www.cyclingforcharityjapan.com  
**Repo:** Static HTML/CSS/JS site hosted on GitHub Pages (custom domain)  
**Contact:** yosuke@cyclingforcharityjapan.com  
**NPO status:** Certified NPO (Tokyo), founded 2015, NPO legal status since 2019  

CFC organises charity cycling rides in Japan for young people (Challengers) from residential care homes (児童養護施設). Guides ride alongside Challengers. 100% of donations since 2019 go directly to ride activities.

---

## Pages

| File | Purpose |
|------|---------|
| `index.html` | Homepage — main entry point |
| `about.html` | Organisation overview, team, annual reports |
| `meet-the-team.html` | Staff and volunteer profiles |
| `get-involved.html` | How to join as Challenger / Guide / Sponsor + Contact form |
| `donate.html` | Donation page (Donorbox embed + bank transfer) |
| `privacy.html` | Privacy policy |
| `404.html` | Custom error page |

---

## Bilingual System (Critical — read carefully)

The site is bilingual Japanese/English. Japanese is the default.

### Class used per page
| Pages | Body class for English |
|-------|----------------------|
| `index.html`, `about.html`, `meet-the-team.html` | `body.en` |
| `get-involved.html`, `donate.html`, `privacy.html` | `body.lang-en` |

### CSS rules
```css
[data-lang="en"] { display: none; }
body.en [data-lang="ja"] { display: none; }
body.en [data-lang="en"] { display: block; }
/* (lang-en pages use body.lang-en instead of body.en) */
```

### HTML pattern — always use this
```html
<span data-lang="ja">日本語テキスト</span>
<span data-lang="en">English text</span>
```

**Critical rules:**
- Never put `style="display:..."` inline on elements that also have `data-lang` — it overrides the CSS toggle
- Use a wrapper `<div data-lang="ja">` / `<div data-lang="en">` (no inline display) when wrapping block elements
- Both language versions must always be present together

### Browser language detection (in every page's `<script>`)
```javascript
const saved = localStorage.getItem('cfc_lang');
if (saved === 'en') {
  document.body.classList.add('en'); // or 'lang-en' depending on page
} else if (!saved) {
  const browserLang = navigator.language || navigator.userLanguage || 'ja';
  if (!browserLang.toLowerCase().startsWith('ja')) {
    document.body.classList.add('en');
  }
}
```

---

## CSS Variables (defined in each page's `<style>`)

```css
:root {
  --green:        #2E7D32;
  --green-light:  #43A047;
  --green-pale:   #E8F5E9;
  --orange:       #E65100;
  --orange-light: #FF6D00;
  --orange-pale:  #FFF3E0;
  --navy:         #1A237E;
  --text:         #1a1a1a;
  --text-light:   #666;
  --bg:           #fff;
  --bg-alt:       #f8f9f8;
  --radius:       16px;
  --shadow:       0 4px 24px rgba(0,0,0,0.08);
}
```

`--navy` is used for the Mentor's Take card on the homepage. Do not omit it.

---

## Homepage Section Order (index.html)

1. NAV
2. MOBILE MENU
3. HERO (primary CTA: Donate, secondary: Get Involved)
4. STATS BAR
5. CHALLENGER STORY (K-kun — see below)
6. MISSION
7. HOW IT WORKS
8. RIDES
9. UPCOMING EVENTS
10. ANNUAL REPORTS
11. BLOG / NEWS (4 cards)
12. GALLERY (collapsible by year)
13. GET INVOLVED
14. PARTNERS
15. CTA
16. FOOTER

---

## Key Stats (homepage stats bar)

| Stat | Value | Note |
|------|-------|------|
| Founded | 2015 | |
| Events completed | 42 | |
| Challenger participations | 75 | のべ人数 (cumulative, not unique individuals) |
| Total raised | ¥15.8M / ¥1,580万 | Includes pre-NPO period — verify if updating |

**Note:** The "100% of donations go to rides" claim is only accurate from 2019 onwards (NPO establishment). Label accordingly.

---

## Challenger Story — K-kun (accurate version)

Used on homepage and donate page. **Do not alter or embellish this story.**

- K was quiet and rarely spoke up
- **Year 1:** Fixed another challenger's flat tyre unprompted → received gratitude → seed of confidence
- **Year 2:** Appointed team mechanic → naturally started talking with more people
- **Year 3:** Became a leader guiding other challengers; organised his own cycling events outside CFC
- **Quote:** 「将来は自転車に関わる仕事をしたい」/ "I want to work with bikes when I grow up."
- **Photo:** `児童養護施設交流会.JPG` (K-kun in blue shirt teaching a child bike maintenance)

---

## Gallery (index.html)

- Collapsible by year — **2026 is open by default**, all older years collapsed
- Collapsed years use `data-src` instead of `src` on `<img>` tags — images only load when expanded
- JavaScript function `toggleYear(label)` handles expand/collapse and triggers `src` swap
- Do not add `loading="lazy"` to collapsed year images (they use `data-src` instead)
- 2026 images use regular `src=` (no data-src)

---

## Annual Reports

| Year | JA PDF | EN PDF | JA Cover | EN Cover |
|------|--------|--------|----------|----------|
| 2025 | `CFC_Annual_Report_2025_JP.pdf` | `CFC_Annual_Report_2025_EN.pdf` | `annual-report-2025-cover-ja.jpg` | `annual-report-2025-cover-en.jpg` |
| 2024 | `CFC_Annual_Report_2024_JP.pdf` | `CFC_Annual_Report_2024_EN.pdf` | `annual-report-2024-cover-ja.jpg` | `annual-report-2024-cover-en.jpg` |
| 2023 | `CFC_Annual_Report_2023_JP.pdf` | `CFC_Annual_Report_2023_EN.pdf` | `annual-report-2023-cover-ja.jpg` | `annual-report-2023-cover-en.jpg` |

Cover images were extracted from the PDFs using `pdftoppm -jpeg -r 150 -f 1 -l 1`.

Annual reports appear on: homepage (after Upcoming Events), about.html (above Testimonials), donate.html (Transparency section).

---

## Donation

- **Donorbox embed:** `https://donorbox.org/embed/cycling-for-charity`
- **Bank:** Rakuten Bank (楽天銀行), Branch: 第二営業支店 (252), Account: 7921638, Name: トクヒ）サイクリングフォーチャリティー
- **Not** a 認定NPO法人 — donations are not tax deductible in Japan
- **Contact email:** yosuke@cyclingforcharityjapan.com

---

## Key Image Files

| File | Description |
|------|-------------|
| `IMG_2215.JPG` | CFC logo / nav image |
| `児童養護施設交流会.JPG` | K-kun challenger story photo |
| `Karuizawa.jpg` | めがね橋 group photo (used in Karuizawa blog card) |
| `annual-report-YYYY-cover-ja/en.jpg` | Report cover images |
| `blog-*.jpg` | Blog card photos |
| `2026-alps-*.jpg` | 2026 North Alps gallery photos |

---

## Analytics

- **GA4 Measurement ID:** G-EXDZ7FDSHQ
- **Property ID:** 538089662
- GA4 tag is in the `<head>` of every page

---

## Blog Cards (homepage)

4 cards, newest first:
1. Virtual Ride 2026 — `blog-virtual-ride-2026.jpg` — 2026年5月11日
2. Spring Tama Lake — `blog-spring-tama-ride.jpg` — 2026年3月30日
3. Karuizawa 2025 — `Karuizawa.jpg` — 2025年10月13日
4. Shimanami 2025 — `blog-shimanami-2025.jpg` — 2025年10月19日

---

## Deployment

- Push to `main` branch → GitHub Pages auto-deploys in ~1 minute
- Custom domain: `www.cyclingforcharityjapan.com` (configured via CNAME)
- No build step — all files are served as-is

```bash
git add <files>
git commit -m "Description of change"
git push
```

---

## Important Rules

1. **Never invent content** — stories, statistics, quotes, or facts about CFC must come from the user or existing verified content on the site. Always ask if unsure.
2. **Always update both languages** — every text change needs a Japanese and English version.
3. **Test the bilingual toggle mentally** — check that `data-lang` wrappers have no conflicting inline `display:` styles.
4. **Stats need verification** — do not update numbers (challengers, events, funds raised) without confirming with the team.
5. **Annual report filenames** — confirm exact PDF filenames before linking; they differ slightly from display names.
