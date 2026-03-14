# Language Preservation Page — Design Spec

## Overview

A dedicated standalone page (`marketing/language-preservation.html`) presenting scanned handwritten Nimíipuutímt vocabulary notes and a typed oral history interview as a digital archive. The page honors a Nimíipuu tribal member who dedicated her life to language preservation and was lost to COVID, while connecting the work to Safe4Seniors' AI-powered language preservation mission.

## Source Documents

- **DOC0112.PDF** (3 pages): Handwritten vocabulary notes — days of the week, seasons, numbers (10–1000), pronouns, verb tenses, and miscellaneous vocabulary. Fading blue ink on yellow lined paper.
- **DOC0114.PDF** (46 pages): Typed oral history interview between Marcus Arthur (interviewer) and Della Davis (~90 years old at time of interview), about life near Spalding, Idaho on the Nez Perce reservation. Permission obtained for publication.

## Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Page type | Separate standalone page | 49 pages of content needs room; keeps marketing page focused |
| Page layout | Hybrid (inline gallery + modal reader) | 3 vocabulary pages work inline; 46-page interview needs a dedicated reader |
| Scan/transcription layout | Side by side (scan left, text right) | Archival standard (Library of Congress pattern); stacks on mobile |
| Visual treatment | Archival Warmth | Cream backgrounds, serif typography, gold accents; bridges brand with reverence |
| Tone | Archival + Living Resource | Let documents speak, then connect to AI mission |
| Attribution | Placeholder until verified | "A Nimíipuu language keeper" — update when name confirmed |
| Transcriptions | Both documents fully transcribed | Full preservation; unclear words marked `[unclear]` |

## Page Structure

### 1. Header/Nav
- Same fixed navigation as main site (`index.html`)
- Includes "Back to Home" link

### 2. Hero Dedication
- Background: deep blue gradient (brand blue `#1e3a5f`)
- Gold badge: "Language Preservation"
- Title: *"Preserving Nimíipuutímt"*
- Subtitle: Context about fewer than 100 fluent speakers remaining
- Attribution placeholder: "A Nimíipuu language keeper"
- Memorial note referencing COVID loss

### 3. Handwritten Vocabulary Section (Inline)
- Cream/parchment background (`#faf8f4`)
- All 3 pages displayed as full-width side-by-side cards
- Each card: scanned image (left) + typed transcription (right)
- Contextual heading per page:
  - Page 1: "Days, Seasons & Vocabulary"
  - Page 2: "Numbers & Counting"
  - Page 3: "Pronouns, Verbs & Tenses"
- Scanned images have subtle drop shadow (physical page on table feel)
- Transcriptions in Georgia serif, organized by topic groupings within each page
- Nez Perce words visually distinguished (gold color or slightly heavier weight)
- On mobile (<768px): stacks vertically (image top, transcription below)
- Fade-up animation on scroll entry

### 4. Oral History Section (Preview + Modal)
- Preview card showing:
  - Context about who Della Davis was and what the interview captures
  - First page of interview (scan image)
  - "Read Full Interview" button (brand gold)
- Button opens full-screen modal viewer

### 5. Full-Screen Interview Reader (Modal)
- Full-viewport overlay with dark backdrop
- Side-by-side: scan left, transcription right
- Page navigation:
  - Previous/next arrow buttons
  - Page number indicator ("Page 12 of 46")
  - Keyboard: left/right arrow keys
- Close: X button (top-right) + Escape key
- On mobile: stacks vertically with previous/next tap buttons (no swipe gestures — keeps vanilla JS simple)
- Page transitions: simple crossfade

### 6. Mission Connection Section
- How Safe4Seniors' AI platform continues this preservation work
- Brief text connecting the handwritten notes and oral history to the AI mission
- Link back to main site mission section

### 7. Footer
- Same footer as main site

## Visual Design

### Color Palette (Archival Warmth)
- Page background: `#faf8f4` (warm cream)
- Scan card area: `#fffde6`
- Transcription area: `#ffffff`
- Header/hero: `#1e3a5f` gradient (brand blue)
- Accent: `#d69e2e` (brand gold) — badges, borders, Nez Perce word highlights
- Body text: `#333333`
- Transcription text: `#4a4a4a`
- Captions/labels: `#8a7a5a`
- Section dividers: `#e0d8b8` (warm tan)

### Typography
- Headings: Georgia, serif
- Body/transcriptions: Georgia, serif
- UI elements (nav, buttons, page indicators): system sans-serif (matches main site)
- Nez Perce words in transcriptions: gold color (`#d69e2e`) or heavier weight

### Interactions & Animation
- Vocabulary cards: fade-up on scroll (existing `IntersectionObserver` pattern)
- Interview modal: slide up from bottom with backdrop fade
- Modal page transitions: crossfade
- Scan image hover: subtle scale (1.01)
- No parallax or heavy effects

## Asset Pipeline

### Image Extraction
- Extract all pages from both PDFs as PNG at 150 DPI
- Directory: `marketing/assets/language-preservation/`
- Naming convention:
  - `vocab-page-1.png`, `vocab-page-2.png`, `vocab-page-3.png`
  - `interview-page-01.png` through `interview-page-46.png`

### Image Optimization
- Compress PNGs after extraction (target ~100-200KB per page, ~5-8MB total for all 49 pages)
- Consider WebP with PNG fallback if total size exceeds 10MB

### Performance
- Vocabulary images: eager load (inline, above fold)
- Interview images: JS-controlled rendering in modal — only the current page + 1 ahead/behind are in the DOM at any time. Pages are inserted/removed as the user navigates. This avoids loading all 46 images at once.

## Transcription Approach
- All 49 pages transcribed
- Handwritten pages: organized by topic groupings (Days, Seasons, Numbers, etc.)
- Interview pages: preserve original M/D dialogue format exactly
- Unclear words: marked with `[unclear]` for human verification
- No editorial changes to content

## Main Site Integration
- Add "Explore the Archive" button to the existing "Nimipuutímt Language Preservation" card (the second card in the `#mission` section, with the `border-t-4 border-brand-gold` top border) in `marketing/index.html`
- Button links to `language-preservation.html`

## Transcription Content Strategy
- All transcription text is embedded directly in the HTML file as structured `<div>` elements within each page's viewer
- Vocabulary pages: small amount of text, no concern
- Interview pages: 46 pages of dialogue text adds ~50-80KB to the HTML — acceptable for a static site with no framework overhead
- Georgia is a web-safe font (pre-installed on all major OS) — no `@font-face` or font file loading needed

## Files Created/Modified
- **New:** `marketing/language-preservation.html` — the full page
- **New:** `marketing/assets/language-preservation/*.png` — 49 page images
- **Modified:** `marketing/index.html` — add link button to mission section card
- **Modified:** `public/` — manually copy updated files from `marketing/` to `public/` (same pattern used for existing site files)

## Out of Scope
- Audio/video content
- Interactive language learning features
- Search/filter functionality within documents
- User-contributed content or comments
- CMS or dynamic content management
