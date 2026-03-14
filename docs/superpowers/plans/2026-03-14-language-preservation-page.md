# Nimíipuutímt Language Preservation Page — Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a standalone archival page presenting scanned Nez Perce language documents with transcriptions, an interactive modal reader, and a connection to the Safe4Seniors AI mission.

**Architecture:** Static HTML page (`marketing/language-preservation.html`) with Tailwind CSS via CDN, vanilla JavaScript for the modal reader. Scanned pages stored as optimized PNGs. All transcription text embedded directly in HTML. No build step, no framework.

**Tech Stack:** HTML5, Tailwind CSS (CDN), vanilla JavaScript, pdftoppm/pngquant for asset extraction

**Spec:** `docs/superpowers/specs/2026-03-14-language-preservation-page-design.md`

**Source PDFs:** `/Users/myers/Downloads/scansnezpercelanguage.zip` containing `DOC0112.PDF` (3 pages, handwritten vocab) + `DOC0114.PDF` (46 pages, typed collection)

**Note on DOC0114 structure:** The 46-page PDF contains multiple documents:
- Pages 1–24: Della Davis conversational interview (M = Marcus Arthur, D = Della Davis)
- Pages 25–27: Janette Wilson interview (March 25, 1973)
- Pages 28–29: "A Short Story" by Almeda Stevens (traditional tale about disobedient children)
- Pages 30–34: "A Short Tale of Instruction" by Dorcas Miller (traditional teaching story)
- Pages 35–41: Anthropology 210 — Nez Perce verb conjugations (to say, to see, to hear, to build, to make, to be sick)
- Page 42: Examples of questions in Nez Perce
- Pages 43–46: Typed vocabulary reference (months, numbers, days, seasons, currency)

**File Structure:**
- `marketing/language-preservation.html` — the complete page (single file, all HTML/CSS/JS inline, following existing site pattern)
- `marketing/assets/language-preservation/*.png` — 49 optimized page images
- `marketing/index.html` — add archive link to mission card (minor edit)
- `public/` — mirror of marketing/ for deployment

---

## Chunk 1: Asset Pipeline

### Task 1: Extract and optimize page images from PDFs

**Files:**
- Create: `marketing/assets/language-preservation/vocab-page-1.png` through `vocab-page-3.png`
- Create: `marketing/assets/language-preservation/collection-page-01.png` through `collection-page-46.png`

- [ ] **Step 1: Create the asset directory**

```bash
mkdir -p marketing/assets/language-preservation
```

- [ ] **Step 2: Extract vocabulary pages from DOC0112.PDF at 150 DPI**

```bash
pdftoppm -png -r 150 /tmp/nezperce_scans/DOC0112.PDF /tmp/nezperce_scans/vocab
```

Expected: Creates `vocab-1.png`, `vocab-2.png`, `vocab-3.png` in `/tmp/nezperce_scans/`

- [ ] **Step 3: Copy and rename vocabulary pages to project**

```bash
cp /tmp/nezperce_scans/vocab-1.png marketing/assets/language-preservation/vocab-page-1.png
cp /tmp/nezperce_scans/vocab-2.png marketing/assets/language-preservation/vocab-page-2.png
cp /tmp/nezperce_scans/vocab-3.png marketing/assets/language-preservation/vocab-page-3.png
```

- [ ] **Step 4: Copy collection pages from already-extracted DOC0114 images**

The pages were already extracted at 150 DPI to `/tmp/nezperce_scans/doc0114_page-*.png`. Copy and rename:

```bash
for i in $(seq -w 1 46); do
  cp /tmp/nezperce_scans/doc0114_page-${i}.png \
     marketing/assets/language-preservation/collection-page-${i}.png
done
```

- [ ] **Step 5: Optimize all PNGs with pngquant**

Target: ~100-200KB per page, ~5-8MB total.

```bash
which pngquant || brew install pngquant
pngquant --quality=65-80 --force --ext .png marketing/assets/language-preservation/*.png
```

- [ ] **Step 6: Verify file sizes**

```bash
du -sh marketing/assets/language-preservation/
ls -la marketing/assets/language-preservation/ | head -5
```

Expected: total ~5-8MB, individual files ~100-200KB each. If total exceeds 10MB, re-run pngquant with lower quality or convert to WebP.

- [ ] **Step 7: Commit assets**

```bash
git add marketing/assets/language-preservation/
git commit -m "assets: add optimized scanned page images for language preservation archive"
```

---

## Chunk 2: Page Scaffold — HTML Structure, Header, Hero, Footer

### Task 2: Create the page with header, hero dedication, and footer

**Files:**
- Create: `marketing/language-preservation.html`

**Context:** Follow the same patterns as `marketing/index.html` — Tailwind CSS via CDN, same nav/footer, same animation classes. Key differences: Georgia serif for content typography, warm cream `#faf8f4` background.

Reference `marketing/index.html` for: `<head>` structure (lines 1-100), navigation (lines 101-160), footer (lines 1000-1040), JavaScript (lines 1040-1067).

- [ ] **Step 1: Create `marketing/language-preservation.html`**

Write the complete HTML file with:
1. `<head>` — meta tags (title: "Preserving Nimíipuutímt | Safe4Seniors"), Tailwind CDN with extended config adding archival colors, `<style>` block with all CSS
2. Navigation — fixed header with "Back to Home" link, logo swap, NVIDIA badge
3. Hero dedication — deep blue gradient, gold badge, title, subtitle, attribution placeholder
4. Empty placeholder `<section>` elements for vocabulary (`#vocabulary`), collection (`#collection`), mission (`#mission-connection`)
5. Empty modal `<div>` (`#reader-modal`) for the full-screen reader
6. Footer — same as index.html
7. `<script>` — scroll handler for logo swap, IntersectionObserver for fade-up

The Tailwind config should extend with these colors:
```javascript
'brand-blue': '#1e3a5f',
'brand-blue-light': '#2c5282',
'brand-gold': '#d69e2e',
'brand-green': '#38a169',
'archival-cream': '#faf8f4',
'archival-scan': '#fffde6',
'archival-tan': '#e0d8b8',
'archival-caption': '#8a7a5a',
```

The `<style>` block should include these custom CSS classes:

**Animation classes** (copied from index.html):
- `.fade-up` / `.fade-up.visible` / `.fade-up-delay-1/2/3`
- `.card-hover`
- `.logo-full` / `.logo-full.hidden-logo` / `.logo-collapsed` / `.logo-collapsed.visible-logo`

**Archival document viewer**:
- `.document-viewer` — CSS grid, 2 columns, collapses to 1 on mobile (<768px), border-radius 12px, overflow hidden, subtle shadow
- `.scan-panel` — background `#fffde6`, padding 16px, img fills width, subtle hover scale(1.01)
- `.transcription-panel` — background white, padding 32px, Georgia serif, color `#4a4a4a`, line-height 1.8, left border `#e0d8b8` (top border on mobile)
- `.transcription-panel .nez-perce` — color `#d69e2e`, font-weight 600
- `.transcription-panel .topic-heading` — small uppercase label, color `#8a7a5a`, spacing above/below

**Modal reader**:
- `.modal-overlay` — fixed inset 0, dark backdrop rgba(0,0,0,0.85), z-index 100, hidden by default, opacity transition
- `.modal-overlay.active` — display flex, centered
- `.modal-overlay.visible` — opacity 1
- `.modal-content` — background `#faf8f4`, 95vw × 90vh, border-radius, flex column, translateY transition
- `.modal-header` — flex row, brand-blue background, white text, close button
- `.modal-body` — flex 1, CSS grid 2 columns (1 on mobile), overflow auto on each panel
- `.modal-footer` — flex center, white background, nav buttons + page indicator
- `.modal-nav-btn` — brand-blue background, white text, rounded, hover lighter, disabled opacity 0.3

**Serif utility**: `.archival-serif` — font-family: Georgia, 'Times New Roman', serif

The hero section HTML:
```html
<section class="pt-24 pb-16 md:pt-32 md:pb-20 px-4 bg-gradient-to-br from-brand-blue via-brand-blue-light to-brand-blue text-white text-center">
    <div class="max-w-4xl mx-auto">
        <span class="inline-block bg-brand-gold text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-4">Language Preservation</span>
        <h1 class="text-3xl md:text-5xl font-bold mb-4 archival-serif fade-up">Preserving Nimíipuutímt</h1>
        <p class="text-blue-200 text-lg md:text-xl max-w-3xl mx-auto mb-6 archival-serif fade-up fade-up-delay-1">
            Fewer than 100 fluent speakers of the Nez Perce language remain. These handwritten notes and recorded conversations are part of a lifetime of work to keep the language alive.
        </p>
        <p class="text-blue-300 text-sm max-w-2xl mx-auto fade-up fade-up-delay-2">
            Created by <span class="text-brand-gold font-semibold">a Nimíipuu language keeper</span> who dedicated her life to this work.
            <br>Lost to COVID — her handwritten notes are among the last physical records of this effort.
        </p>
    </div>
</section>
```

- [ ] **Step 2: Verify the page loads in browser**

```bash
open marketing/language-preservation.html
```

Expected: Blue hero with title, placeholder sections, footer. "Back to Home" link in nav works.

- [ ] **Step 3: Commit scaffold**

```bash
git add marketing/language-preservation.html
git commit -m "feat: add language preservation page scaffold with hero and navigation"
```

---

## Chunk 3: Vocabulary Section — Inline Side-by-Side Cards with Transcriptions

### Task 3: Build the three vocabulary page viewers with transcriptions

**Files:**
- Modify: `marketing/language-preservation.html` — fill in the `#vocabulary` section

**Context:** Each handwritten page gets a side-by-side card (scan left, transcription right). Read each page image carefully and transcribe all visible text. The handwritten pages contain:
- Page 1: Days of the week, seasons, vocabulary (railroad train, stop, teach, fire, forearm)
- Page 2: Numbers 10-1000, currency terms
- Page 3: Pronouns, verbs, tenses

- [ ] **Step 1: Read all 3 vocabulary page images for transcription**

Read each image at `marketing/assets/language-preservation/vocab-page-*.png`. Take detailed notes. Use `[unclear]` for any text that cannot be confidently read.

- [ ] **Step 2: Add section header and three document-viewer cards to `#vocabulary`**

Structure for the section:

```html
<div class="text-center mb-12">
    <span class="inline-block bg-brand-gold text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-3">Handwritten Notes</span>
    <h2 class="text-2xl md:text-3xl font-bold text-brand-blue archival-serif mb-3 fade-up">The Vocabulary Pages</h2>
    <p class="text-archival-caption max-w-2xl mx-auto archival-serif fade-up fade-up-delay-1">
        Three pages of handwritten notes in fading blue ink — days of the week, numbers, pronouns, and verb forms in Nimíipuutímt.
    </p>
</div>
```

For each of the 3 pages, create a card with this structure:

```html
<div class="mb-12 fade-up">
    <h3 class="text-lg font-bold text-brand-blue archival-serif mb-4">[Page Title]</h3>
    <div class="document-viewer">
        <div class="scan-panel">
            <img src="./assets/language-preservation/vocab-page-N.png"
                 alt="[Descriptive alt text]">
        </div>
        <div class="transcription-panel">
            <div class="topic-heading">[Topic Name]</div>
            <p>[English] — <span class="nez-perce">[Nez Perce word]</span></p>
            <!-- ... all entries for this topic ... -->
            <div class="topic-heading">[Next Topic]</div>
            <!-- ... etc ... -->
        </div>
    </div>
</div>
```

Page titles: "Days, Seasons & Vocabulary", "Numbers & Counting", "Pronouns, Verbs & Tenses"

The transcription content must be complete — every visible word on each page, organized by topic. Use `<span class="nez-perce">word</span>` for all Nez Perce words.

- [ ] **Step 3: Verify vocabulary section renders correctly**

```bash
open marketing/language-preservation.html
```

Expected: Three side-by-side cards. Scan images left, formatted transcriptions right. On mobile (<768px): stacks vertically. Fade-up animation on scroll.

- [ ] **Step 4: Commit vocabulary section**

```bash
git add marketing/language-preservation.html
git commit -m "feat: add inline vocabulary section with transcriptions for 3 handwritten pages"
```

---

## Chunk 4: Collection Section — Preview Card + Modal Reader

### Task 4: Build the collection preview and full-screen modal reader

**Files:**
- Modify: `marketing/language-preservation.html` — fill in `#collection` section and `#reader-modal`, add reader JS

**Context:** The 46-page collection needs a preview card with a contents overview, then a full-screen modal reader. The modal uses JS to render only the current page (with adjacent page preloading via `new Image()`).

**Important — DOM approach for modal:** All 46 page containers (scan + transcription) are pre-rendered as hidden `<div>` elements with `id="page-N"` inside a `<template>` or hidden container. The JS shows/hides pages by toggling a CSS class rather than using innerHTML. This avoids any dynamic HTML injection.

- [ ] **Step 1: Read all 46 collection page images for transcription**

Read every page at `marketing/assets/language-preservation/collection-page-*.png`. This is the most time-intensive step. For each page, transcribe the full text preserving:
- M/D dialogue format for interviews (pages 1-24)
- Narrative paragraphs for stories (pages 28-34)
- Table/list format for vocabulary and conjugations (pages 35-46)
- All Nez Perce words wrapped in `<span class="nez-perce">`

- [ ] **Step 2: Add the collection section HTML**

Fill in `#collection` with:
1. Section header with gold badge "The Collection", title "Oral Histories & Language Records"
2. Grid of 5 overview cards describing the collection contents (Della Davis Interview, Janette Wilson Interview, Traditional Stories, Verb Conjugations, Questions & Vocabulary) — each with page range and brief description
3. Preview document-viewer card showing page 1 with first few lines of transcription
4. "Read Full Collection" button that calls `openReader(1)`

- [ ] **Step 3: Build the modal reader**

Fill in `#reader-modal` with:
1. `.modal-content` container with header (title + close button), body (scan + transcription panels), footer (prev/next + page indicator)
2. Inside `.modal-body`: a hidden container `<div id="reader-pages" class="hidden">` holding all 46 page divs, each with `id="reader-page-N"` containing:
   - An `<img>` tag pointing to the collection page image
   - A `<div>` with the full transcription HTML

Structure for each page inside the hidden container:
```html
<div id="reader-page-1" class="reader-page">
    <div class="reader-page-scan">
        <img src="./assets/language-preservation/collection-page-01.png"
             alt="Collection page 1" loading="lazy">
    </div>
    <div class="reader-page-text">
        <div class="topic-heading">A Conversational Interview with Della Davis of Spalding</div>
        <p style="font-style:italic;color:#8a7a5a;">(Marcus Arthur) M – will be Interviewer<br>(Della Davis) D – will be Interviewee</p>
        <p><strong>M</strong> – Aunt Elizabeth was your good friend wasn't she?</p>
        <p><strong>D</strong> – Yes—we grew up together.</p>
        <!-- ... full page 1 transcription ... -->
    </div>
</div>
```

- [ ] **Step 4: Add the modal reader JavaScript**

Add to the `<script>` block. The JS approach:
- `openReader(page)` — shows modal overlay, calls `renderPage()`
- `closeReader()` — hides modal, restores body scroll
- `readerNav(delta)` — increments/decrements page, calls `renderPage()`
- `renderPage()` — clones the scan `<img>` and transcription `<div>` from the hidden `#reader-page-N` element into the visible `#reader-scan` and `#reader-transcription` panels using `cloneNode(true)`. Updates page indicator and nav button disabled states. Preloads adjacent images with `new Image()`.
- Keyboard listener: ArrowLeft/ArrowRight for nav, Escape to close
- Backdrop click listener to close

```javascript
const TOTAL_PAGES = 46;
let currentPage = 1;

function openReader(page) {
    currentPage = page;
    const modal = document.getElementById('reader-modal');
    modal.classList.add('active');
    modal.setAttribute('aria-hidden', 'false');
    document.body.style.overflow = 'hidden';
    requestAnimationFrame(() => {
        requestAnimationFrame(() => modal.classList.add('visible'));
    });
    renderPage();
}

function closeReader() {
    const modal = document.getElementById('reader-modal');
    modal.classList.remove('visible');
    modal.setAttribute('aria-hidden', 'true');
    setTimeout(() => {
        modal.classList.remove('active');
        document.body.style.overflow = '';
    }, 300);
}

function readerNav(delta) {
    const newPage = currentPage + delta;
    if (newPage < 1 || newPage > TOTAL_PAGES) return;
    currentPage = newPage;
    renderPage();
}

function renderPage() {
    const source = document.getElementById('reader-page-' + currentPage);
    const scanDest = document.getElementById('reader-scan');
    const textDest = document.getElementById('reader-transcription');

    // Clear and clone content from hidden source
    scanDest.replaceChildren(source.querySelector('.reader-page-scan').cloneNode(true));
    textDest.replaceChildren(source.querySelector('.reader-page-text').cloneNode(true));
    scanDest.scrollTop = 0;
    textDest.scrollTop = 0;

    // Update indicators
    const label = 'Page ' + currentPage + ' of ' + TOTAL_PAGES;
    document.getElementById('reader-title').textContent = label;
    document.getElementById('reader-page-indicator').textContent = label;
    document.getElementById('reader-prev').disabled = (currentPage === 1);
    document.getElementById('reader-next').disabled = (currentPage === TOTAL_PAGES);

    // Preload adjacent images
    [currentPage - 1, currentPage + 1].forEach(function(p) {
        if (p >= 1 && p <= TOTAL_PAGES) {
            var img = new Image();
            var padded = String(p).padStart(2, '0');
            img.src = './assets/language-preservation/collection-page-' + padded + '.png';
        }
    });
}

document.addEventListener('keydown', function(e) {
    var modal = document.getElementById('reader-modal');
    if (!modal.classList.contains('active')) return;
    if (e.key === 'ArrowLeft') readerNav(-1);
    if (e.key === 'ArrowRight') readerNav(1);
    if (e.key === 'Escape') closeReader();
});

document.getElementById('reader-modal').addEventListener('click', function(e) {
    if (e.target === e.currentTarget) closeReader();
});
```

- [ ] **Step 5: Fill all 46 page transcriptions**

Write all 46 `<div id="reader-page-N">` elements in the hidden container, each with complete transcription from reading the page image. This is the most labor-intensive step. Follow these rules:
- Interview dialogue: `<p><strong>M</strong> – text</p>` / `<p><strong>D</strong> – text</p>`
- Section headers: `<div class="topic-heading">Title</div>`
- Nez Perce words: `<span class="nez-perce">word</span>`
- Unclear text: `[unclear]`
- Preserve original paragraph breaks and formatting intent

- [ ] **Step 6: Verify modal reader works**

Open the page and test:
1. Click "Read Full Collection" — modal opens with slide-up animation
2. Page 1 shows correct scan and transcription side by side
3. Click Next — advances to page 2 with correct content
4. Click Previous on page 1 — button is disabled
5. Navigate to page 46 — Next button is disabled
6. Press left/right arrow keys — keyboard navigation works
7. Press Escape — modal closes with fade-out
8. Click dark backdrop — modal closes
9. Resize to mobile width — modal body stacks vertically
10. Scroll within transcription panel — works independently of scan panel

- [ ] **Step 7: Commit collection section and reader**

```bash
git add marketing/language-preservation.html
git commit -m "feat: add collection preview and modal reader with 46-page transcriptions"
```

---

## Chunk 5: Mission Connection, Main Site Integration, Deploy

### Task 5: Add mission connection section

**Files:**
- Modify: `marketing/language-preservation.html` — fill in `#mission-connection`

- [ ] **Step 1: Add mission connection content**

```html
<div class="max-w-4xl mx-auto text-center">
    <span class="inline-block bg-brand-gold text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wide mb-3">The Mission Continues</span>
    <h2 class="text-2xl md:text-3xl font-bold text-brand-blue archival-serif mb-6 fade-up">From Handwritten Notes to AI</h2>
    <div class="archival-serif text-gray-600 leading-relaxed max-w-3xl mx-auto space-y-4 fade-up fade-up-delay-1">
        <p>
            The pages you see here represent decades of painstaking work — one person writing down words, recording conversations, preserving verb forms and stories that exist nowhere else.
        </p>
        <p>
            Safe4Seniors is building AI that can hold conversations in Nimíipuutímt. The same platform that makes daily wellness calls to seniors aging in place can help keep an endangered language alive — not as a museum artifact, but as a living, spoken language.
        </p>
        <p>
            Fewer than 100 fluent speakers remain. Every word matters.
        </p>
    </div>
    <a href="./index.html#mission" class="inline-block mt-8 bg-brand-blue text-white font-bold px-8 py-3 rounded-xl shadow-lg hover:shadow-xl hover:-translate-y-0.5 transition-all fade-up fade-up-delay-2">
        Learn About Our Mission
    </a>
</div>
```

- [ ] **Step 2: Commit**

```bash
git add marketing/language-preservation.html
git commit -m "feat: add mission connection section to language preservation page"
```

### Task 6: Add link from main site mission card

**Files:**
- Modify: `marketing/index.html`

- [ ] **Step 1: Add archive button to mission card**

In `marketing/index.html`, find the "Nimipuutímt Language Preservation" card in the `#mission` section (the second card, with `border-t-4 border-brand-gold`, around line 771). After the closing `</p>` tag that contains "Fewer than 100 fluent Nimipuutímt speakers remain.", add:

```html
<a href="./language-preservation.html" class="inline-block mt-4 bg-brand-gold text-white text-sm font-bold px-5 py-2 rounded-lg hover:shadow-lg hover:-translate-y-0.5 transition-all">
    Explore the Archive →
</a>
```

- [ ] **Step 2: Verify link works from main site**

```bash
open marketing/index.html
```

Scroll to "Two Missions. One Platform." section. Gold-bordered card should have "Explore the Archive →" button. Clicking it navigates to the language preservation page.

- [ ] **Step 3: Commit**

```bash
git add marketing/index.html
git commit -m "feat: add archive link to mission section for language preservation page"
```

### Task 7: Deploy to public directory

**Files:**
- Copy: `marketing/index.html` → `public/index.html`
- Copy: `marketing/language-preservation.html` → `public/language-preservation.html`
- Copy: `marketing/assets/language-preservation/` → `public/assets/language-preservation/`

- [ ] **Step 1: Copy files to public**

```bash
cp marketing/index.html public/index.html
cp marketing/language-preservation.html public/language-preservation.html
cp -r marketing/assets/language-preservation public/assets/language-preservation
```

- [ ] **Step 2: Verify**

```bash
ls public/language-preservation.html
ls public/assets/language-preservation/ | wc -l
diff marketing/language-preservation.html public/language-preservation.html
```

Expected: 49 image files, no diff between marketing and public HTML.

- [ ] **Step 3: Commit**

```bash
git add public/language-preservation.html public/assets/language-preservation/ public/index.html
git commit -m "deploy: copy language preservation page and assets to public directory"
```

- [ ] **Step 4: Final walkthrough**

```bash
open public/language-preservation.html
```

1. Page loads with blue hero dedication
2. Scroll — vocabulary cards appear with fade-up, showing scan + transcription side by side
3. Scroll to collection — overview cards visible, preview card shows page 1
4. Click "Read Full Collection" — modal opens
5. Navigate through pages with buttons and keyboard arrows
6. Close modal (X, Escape, or backdrop click)
7. Scroll to mission section — "Learn About Our Mission" links back to main site
8. On main site `#mission` section — "Explore the Archive" button navigates to this page
