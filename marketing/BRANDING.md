# Safe4Seniors Branding Assets

## SVG Files Created

### 1. safe4seniors-wordmark.svg (320x80)
Full logo with decorative lines and tagline "AI VOICE COMPANIONS"
- **Usage:** Main landing page header, marketing materials
- **Colors:** Teal (#0D9488), Gold (#D4A853), Slate (#1E293B), Gray (#64748B)

### 2. safe4seniors-mark.svg (90x40)
Collapsed "Safe4" mark in gold box
- **Usage:** Scrolled header state, mobile header, compact spaces
- **Colors:** Gold border (#D4A853), Teal "4" (#0D9488), Slate text (#1E293B)

### 3. safe4seniors-icon.svg (200x200)
Circular shield icon with stylized hands/protection symbol
- **Usage:** Favicon, app icons, social media profiles
- **Colors:** Teal gradient circle, Light teal shield, Gold hands

## Brand Colors

```css
--teal: #0D9488;           /* Primary brand color */
--gold: #D4A853;           /* Accent/warmth */
--slate: #1E293B;          /* Text/dark elements */
--light-teal: #CCFBF1;     /* Backgrounds/highlights */
--gray: #64748B;           /* Subtle text/borders */
```

## Scroll-Collapsing Header Implementation

### Behavior
- **Desktop (≥640px):** Full wordmark at top, collapses to "Safe4" mark after 50px scroll
- **Mobile (<640px):** Always shows collapsed mark (space-saving)
- **Transition:** 250ms smooth fade + scale using CSS transforms

### Files Modified
- `website/index.html` - Added inline SVGs, scroll detection JS
- `website/favicon.svg` - Replaced with new branded icon

### Technical Details
- Uses `requestAnimationFrame` for 60fps scroll performance
- Position absolute swapping prevents layout shift
- GPU-accelerated transitions (opacity + transform)
- Throttled scroll listener to reduce CPU load

## Testing
```bash
# View in browser
open website/index.html

# Or from repo root
cd /Users/myers/src/github.com/aging-in-place
open website/index.html
```

Scroll down to see logo collapse, scroll to top to see expansion.

## NVIDIA Inception Badge

### Badge File
- `nvidia-inception-badge.png` (636x280, optimized for web)
- **Source:** Official NVIDIA Inception program assets
- **Link:** https://www.nvidia.com/en-us/startups/

### Header Placement
- **Desktop (≥640px):** Shows between Safe4Seniors logo and navigation
- **On scroll:** Badge fades out when header collapses (after 50px)
- **Mobile (<640px):** Hidden in header (shown in footer only)
- **Styling:** 40px height, subtle opacity hover effect

### Footer Placement
- **Label:** "Proud Member Of" above badge
- **Badge:** 48px height, centered
- **Clear space:** 10px+ all sides (dark background compliance)
- **Clickable:** Links to NVIDIA Inception program page

### Brand Compliance
✅ Maintained clear space on all sides (10px minimum)
✅ Placed on clean white (header) and dark gray (footer) backgrounds
✅ No distortion or stretching
✅ Badge is smaller than Safe4Seniors logo (proper visual hierarchy)

## Deployment
SVG assets and NVIDIA badge are copied to `safe4seniors.ai/public/` for Worker deployment.
