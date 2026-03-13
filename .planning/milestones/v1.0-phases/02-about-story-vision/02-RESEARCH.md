# Phase 02: About / Story / Vision — Research

**Researched:** 2026-03-13
**Domain:** Static HTML/CSS — new page section, image assets, nav integration
**Confidence:** HIGH

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Section Placement**
- New standalone scroll-snap page-section: `<div class="page" id="pageAbout">`
- Positioned between `#pageHome` and `#pageHardware` in the DOM
- Add "About" nav link to both `.nav-links` (desktop) and `.nav-overlay-links` (mobile overlay)

**Manifesto vs Bio Structure**
- Manifesto leads — section opens with the big quote, then bio follows:
  1. Full-width giant manifesto quote: "Most musicians spend years learning theory just to play a chord. We think that's backwards."
  2. Brief mission expansion (1-2 sentences: "remove the barrier between people and harmony")
  3. Founder bio (3-4 sentences) + portrait photo alongside

**Visual Treatment**
- Manifesto quote: Syne font-weight 800, ~5-7vw, white text on navy-deep background
- Teal (#2ae8c4) accent on one key word (e.g. "backwards" or "harmony")
- Section background: `var(--navy-deep)` matching hardware/impressions pages
- Bio section: two-column layout — text left, photo right (stacks vertically on mobile)

**Founder Bio Copy (use verbatim)**
"Dimitri Dähler is a jazz trumpet player, producer, and sound engineer trained at the Hochschule Luzern. The origin of Dimea Arcade was a single frustration: 'I wanted chords to improvise over on my trumpet. People told me modular can't do chords. So I built one that could.' That modular rig became his masterproject performance at HSLU. The audience called it mesmerizing."

Pull-quote below bio: "I was at exactly the place I'd been searching for years."

**Founder Photo**
- Use real photo: `Bild 2.JPG` from BA Konzert folder
- Source: `C:/Users/Milosh Xhavid/Desktop/Milosh Project/BA Konzert/Bild 2.JPG`
- Copy to: `assets/founder-dimitri.jpg`
- Treatment: portrait crop (3:4 or 4:5 aspect ratio), `object-fit: cover`
- Subtle teal border or no border — Claude's discretion based on what looks good

**Hardware Origin Story (02-02)**
- Add short paragraph ABOVE existing `.hw-origin` strip (between `.hw-header` and `.hw-origin`)
  - Opens: "This didn't start as a product. It started as a problem."
  - Brief expansion (2-3 sentences from PROJECT.md founder story, setting up the modular years)
- Keep existing 3-step strip (Modular → Plugin → Hardware) exactly as-is
- Keep existing captions as-is

**Fix Broken Image Paths (02-02)**

| Current broken path | Desktop source | New asset path |
|---|---|---|
| `/mnt/user-data/uploads/WhatsApp_Image_2026-01-17_at_00_04_34.jpeg` | `C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/WhatsApp Image 2026-01-17 at 00.04.34.jpeg` | `assets/hw-origin-performance.jpg` |
| `/mnt/user-data/uploads/Dimeola_Schalltplan.jpg` | `C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/Dimeola Schalltplan.jpg` | `assets/hw-origin-schematic.jpg` |

Both images stay in current HTML positions — update `src` attributes only.

### Claude's Discretion
- Exact aspect ratio of photo container (3:4 vs 4:5)
- Whether manifesto quote uses one-word teal accent or a phrase
- Spacing and padding values within the section
- Whether bio text and photo are on equal-width columns or asymmetric

### Deferred Ideas (OUT OF SCOPE)
None — discussion stayed within phase scope.
</user_constraints>

---

## Summary

Phase 02 is a pure HTML/CSS authoring task with no new framework dependencies. It adds one new `<div class="page" id="pageAbout">` section between the existing `#pageHome` and `#pageHardware` blocks, integrates it into both nav lists, and makes targeted additions to the hardware page (a founder-voiced intro paragraph above `.hw-origin` plus two fixed image paths).

All content is locked in CONTEXT.md and COPY.md. All source images are confirmed present on the local filesystem. The project has no build step — the deliverable is a single modified `index.html` plus three new files in `assets/`. Every visual pattern needed (section structure, two-column grid, manifesto-style quote, pull-quote block, `.section-label`, responsive breakpoints) already exists in the codebase and can be followed directly.

The only novel CSS needed is for the `.about-section` itself: the giant manifesto quote typography and the bio+photo two-column layout. Both follow exact precedents in `.hw-vision-grid` and `.hw-vision-quote` respectively.

**Primary recommendation:** Follow existing section patterns exactly. Write new CSS classes prefixed `.about-` to avoid collisions; borrow spacing, grid, and typography values from `.hw-section` and `.hw-vision`.

---

## Standard Stack

### Core
| Item | Version/Value | Purpose | Source |
|------|--------------|---------|--------|
| Single `index.html` | Current file | All site markup and CSS live here | Project decision |
| Syne font | 800 weight (already loaded) | Manifesto quote headline | Line 8, already in `<link>` |
| DM Mono font | already loaded | Section label, pull-quote attribution | Line 8, already in `<link>` |
| Space Grotesk | already loaded | Body/bio copy | Line 8, already in `<link>` |
| CSS variables | already defined in `:root` | Colors, borders — reuse, don't redefine | Lines 11-19 |

### Supporting
| Item | Value | Purpose | When to Use |
|------|-------|---------|-------------|
| `assets/` directory | exists, one file | Static image storage | Copy all three images here |
| Windows `copy` via shell | PowerShell/cp | Move source images into assets | Once per image, before HTML references them |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Inline CSS in `<style>` block | Separate CSS file | Project uses single-file — keep consistent |
| `<picture>` with WebP | Plain `<img>` with JPEG | JPEG is fine for a single-file prototype site at this stage |

**No installation required.** This phase adds zero npm packages and zero external dependencies.

---

## Architecture Patterns

### DOM Structure for New About Section

The new section inserts at line 759 (after `</div>` closing `#pageHome`, before the `<!-- HARDWARE PAGE -->` comment):

```html
<!-- ==================== ABOUT PAGE ==================== -->
<div class="page" id="pageAbout">
<section class="about-section">
  <div class="about-inner">

    <!-- MANIFESTO -->
    <div class="about-manifesto">
      <div class="section-label">About</div>
      <blockquote class="about-quote">
        Most musicians spend years learning theory just to play a chord.
        We think that's <span class="about-quote-accent">backwards.</span>
      </blockquote>
      <p class="about-mission">...</p>
    </div>

    <!-- BIO + PHOTO -->
    <div class="about-bio-grid">
      <div class="about-bio-text">
        <p class="about-bio-body">...</p>
        <blockquote class="about-pull-quote">
          <span class="about-pull-mark">"</span>
          <div>
            <p class="about-pull-text">I was at exactly the place I'd been searching for years.</p>
            <div class="about-pull-attr">— Dimitri Dähler</div>
          </div>
        </blockquote>
      </div>
      <div class="about-photo-wrap">
        <img src="assets/founder-dimitri.jpg" alt="Dimitri Dähler" class="about-photo">
      </div>
    </div>

  </div>
</section>
</div>
```

### Hardware Page Insertion for 02-02

Insert between `</div>` (closing `.hw-header`, line 772) and `<!-- ORIGIN STRIP -->` (line 774):

```html
<!-- ── FOUNDER INTRO ─────────────────────────────────────────── -->
<div class="hw-founder-intro">
  <p class="hw-founder-voice">This didn't start as a product. It started as a problem.</p>
  <p class="hw-founder-expand">...</p>
</div>
```

### CSS Pattern: Section Container

Follow `.hw-section` exactly:

```css
/* Source: index.html line 207 */
.about-section {
  position: relative;
  padding: 130px 52px 100px;
  background: var(--navy-deep);
  overflow: hidden;
}
.about-inner {
  position: relative;
  max-width: 1200px;
  margin: 0 auto;
}
@media(max-width: 900px) {
  .about-section { padding: 100px 28px 80px; }
}
```

### CSS Pattern: Manifesto Quote

Syne 800, `clamp()` for responsive sizing, teal accent span:

```css
.about-quote {
  font-family: 'Syne', sans-serif;
  font-weight: 800;
  font-size: clamp(32px, 5.5vw, 72px);
  line-height: 1.1;
  color: var(--text);
  letter-spacing: -0.02em;
  max-width: 900px;
  margin-bottom: 28px;
  quotes: none;
  border: none;
  padding: 0;
}
.about-quote-accent {
  color: var(--teal);
}
```

### CSS Pattern: Bio + Photo Two-Column Grid

Follow `.hw-vision-grid` (line 435):

```css
/* Source: index.html line 435 */
.about-bio-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;  /* or asymmetric: 3fr 2fr — Claude's discretion */
  gap: 64px;
  align-items: start;
  margin-top: 72px;
  padding-top: 72px;
  border-top: 1px solid var(--border);
}
@media(max-width: 900px) {
  .about-bio-grid {
    grid-template-columns: 1fr;
    gap: 40px;
  }
  /* Photo moves below text on mobile */
}
```

### CSS Pattern: Founder Photo

```css
.about-photo-wrap {
  position: relative;
}
.about-photo {
  width: 100%;
  aspect-ratio: 3 / 4;   /* or 4/5 — Claude's discretion */
  object-fit: cover;
  object-position: center top;
  display: block;
}
```

### CSS Pattern: Pull-Quote Block

Follow `.hw-vision-quote` (line 446):

```css
/* Source: index.html line 446 */
.about-pull-quote {
  background: rgba(42,232,196,0.03);
  border-left: 2px solid rgba(42,232,196,0.35);
  padding: 28px 36px;
  display: flex;
  gap: 16px;
  align-items: flex-start;
  margin-top: 32px;
}
.about-pull-mark {
  font-family: 'Syne', sans-serif;
  font-size: 52px;
  font-weight: 900;
  color: rgba(42,232,196,0.25);
  line-height: 0.8;
  flex-shrink: 0;
  margin-top: -4px;
}
.about-pull-text {
  font-size: 15px;
  line-height: 1.75;
  color: var(--text-dim);
  font-style: italic;
  margin-bottom: 10px;
}
.about-pull-attr {
  font-family: 'DM Mono', monospace;
  font-size: 9px;
  letter-spacing: .2em;
  text-transform: uppercase;
  color: var(--text-faint);
}
```

### Nav Integration Pattern

Desktop nav (line 634-639) — add after existing `<li>` items, or second position (after Plugin):
```html
<!-- Add as second item in .nav-links, after Plugin -->
<li><a href="#pageAbout" id="navAbout">About</a></li>
```

Mobile overlay (lines 4001-4006) — add matching entry:
```html
<li><a href="#pageAbout">About</a></li>
```

Both nav lists use identical `<li><a href="...">Label</a></li>` pattern — no JavaScript changes needed for basic link behavior.

### Anti-Patterns to Avoid
- **Don't create new CSS variables:** All brand colors are already defined in `:root`. Use `var(--navy-deep)`, `var(--teal)`, `var(--text-dim)` etc.
- **Don't use `font-size` in px for the manifesto quote without `clamp()`:** Every heading in the codebase uses `clamp()` for responsive scaling.
- **Don't add JavaScript** for this section — pure HTML/CSS, no interactivity needed.
- **Don't alter the `.hw-origin` strip** — the 3-step origin strip content is locked and must remain identical.
- **Don't use absolute paths** for images in `src` attributes — use relative `assets/filename.jpg`.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Responsive image sizing | Custom JS resize logic | `aspect-ratio` + `object-fit: cover` in CSS | Native CSS, supported in all modern browsers |
| Two-column → single-column on mobile | JS media query listener | `@media(max-width:900px)` CSS breakpoint | Project already uses this breakpoint consistently at lines 308, 335, 352, 450, 464 |
| Photo file format conversion | Canvas/JS conversion | Copy JPEG as-is, rename to `.jpg` | JPEGs from source are fine for a portfolio/product site |

---

## Common Pitfalls

### Pitfall 1: Image File Copy — Windows Path with Spaces
**What goes wrong:** `cp` or `copy` commands fail silently or partially when source path contains spaces (`Milosh Xhavid`, `BA Konzert`, `Bild 2.JPG`).
**Why it happens:** Shell tokenizes unquoted paths on spaces.
**How to avoid:** Always quote both source and destination fully. Use PowerShell `Copy-Item` or the bash `cp` with full quoted paths.
**Warning signs:** `assets/founder-dimitri.jpg` doesn't appear, or is 0 bytes.

```bash
# Correct — use quoted paths
cp "C:/Users/Milosh Xhavid/Desktop/Milosh Project/BA Konzert/Bild 2.JPG" \
   "C:/Users/Milosh Xhavid/dimea-website/assets/founder-dimitri.jpg"

cp "C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/WhatsApp Image 2026-01-17 at 00.04.34.jpeg" \
   "C:/Users/Milosh Xhavid/dimea-website/assets/hw-origin-performance.jpg"

cp "C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/Dimeola Schalltplan.jpg" \
   "C:/Users/Milosh Xhavid/dimea-website/assets/hw-origin-schematic.jpg"
```

### Pitfall 2: Nav Position — About Link Ordering
**What goes wrong:** Adding "About" at the end of the nav list creates a confusing nav order (Plugin, Hardware, Impressions, Shop, App, About) that doesn't reflect the page scroll order.
**Why it happens:** Copying pattern without thinking about UX intent.
**How to avoid:** Insert as second item in both nav lists (Home → Plugin → **About** → Hardware → Impressions → Shop → App), matching DOM order.

### Pitfall 3: Broken `src` in hw-origin After Fix
**What goes wrong:** New `src` paths use wrong relative depth (e.g. `../assets/` or `/assets/`) instead of `assets/`.
**Why it happens:** The HTML file is at root, so `assets/filename.jpg` is correct — but it's easy to second-guess this.
**How to avoid:** Check existing working image: `assets/arcade-chord-control.png` (line 739) uses `assets/` directly. Follow that pattern exactly.

### Pitfall 4: Photo aspect-ratio Overflow
**What goes wrong:** Portrait photo (3:4) causes layout shift or overflows its grid column on mobile.
**Why it happens:** `aspect-ratio` + `width:100%` on a constrained grid column works fine — but only if `overflow:hidden` or a containing wrapper handles it.
**How to avoid:** Wrap the `<img>` in `.about-photo-wrap` with `overflow:hidden`. Set `width:100%` on the img, not a fixed px height.

### Pitfall 5: Manifesto Quote `<blockquote>` Default Styles
**What goes wrong:** Browser default `blockquote` styles add margin and sometimes a quotation mark or border-left.
**Why it happens:** CSS reset in this file (`*, *::before, *::after { margin: 0; padding: 0 }`) handles margin/padding — but not `quotes` or `border`.
**How to avoid:** Set `quotes: none; border: none; padding: 0; margin: 0;` explicitly on `.about-quote`, or use a `<p>` tag instead.

---

## Code Examples

### Verified Existing Section Pattern (hw-section)
```css
/* Source: index.html line 207 */
.hw-section { position:relative; padding:130px 52px 100px; background:var(--navy-deep); overflow:hidden; }
.hw-inner { position:relative; max-width:1200px; margin:0 auto; }
.hw-header { max-width:700px; margin-bottom:56px; }
```

### Verified section-label Pattern
```css
/* Source: index.html line 152-153 */
.section-label { display:inline-flex; align-items:center; gap:10px; font-family:'DM Mono',monospace;
  font-size:9px; letter-spacing:.4em; text-transform:uppercase; color:var(--teal); margin-bottom:20px; }
.section-label::before { content:''; width:24px; height:1px; background:var(--teal); opacity:.5; }
```

### Verified Pull-Quote Pattern
```css
/* Source: index.html line 446-449 */
.hw-vision-quote { background:rgba(42,232,196,0.03); border-left:2px solid rgba(42,232,196,0.35);
  padding:32px 40px; display:flex; gap:20px; align-items:flex-start; }
.hw-vision-quote-mark { font-family:'Syne',sans-serif; font-size:56px; font-weight:900;
  color:rgba(42,232,196,0.25); line-height:.8; flex-shrink:0; margin-top:-4px; }
.hw-vision-quote-text { font-size:15px; line-height:1.75; color:var(--text-dim); font-style:italic; margin-bottom:12px; }
.hw-vision-quote-attr { font-family:'DM Mono',monospace; font-size:9px; letter-spacing:.2em;
  text-transform:uppercase; color:var(--text-faint); }
```

### Verified Two-Column Grid with Mobile Collapse
```css
/* Source: index.html line 435, 450 */
.hw-vision-grid { display:grid; grid-template-columns:1fr 1fr; gap:64px; margin-bottom:56px; align-items:start; }
@media(max-width:900px) { .hw-vision-grid { grid-template-columns:1fr; gap:40px; } }
```

### Verified clamp() Heading Pattern
```css
/* Source: index.html line 68 */
.hero-title { font-family:'Syne',sans-serif; font-size:clamp(52px,7.5vw,96px); font-weight:800;
  line-height:.95; letter-spacing:-.02em; }
```

### Verified Nav Link Pattern
```html
<!-- Source: index.html lines 634-639 -->
<ul class="nav-links">
  <li><a href="#plugins" id="navPlugins">Plugin</a></li>
  <li><a href="#pageHardware" id="navHardware">Hardware</a></li>
  ...
</ul>
<!-- Mobile overlay: index.html lines 4000-4006 -->
<ul class="nav-overlay-links">
  <li><a href="#plugins">Plugin</a></li>
  <li><a href="#pageHardware">Hardware</a></li>
  ...
</ul>
```

---

## Approved Copy (Verbatim — from COPY.md and CONTEXT.md)

### Manifesto Quote (locked)
"Most musicians spend years learning theory just to play a chord. We think that's backwards."

### Mission Expansion (from COPY.md)
"Harmony shouldn't be a gate you pass through after decades of practice. It should be a playground — something you feel, explore, and own from the moment you touch it."

### Founder Bio (locked — short version from CONTEXT.md)
"Dimitri Dähler is a jazz trumpet player, producer, and sound engineer trained at the Hochschule Luzern. The origin of Dimea Arcade was a single frustration: 'I wanted chords to improvise over on my trumpet. People told me modular can't do chords. So I built one that could.' That modular rig became his masterproject performance at HSLU. The audience called it mesmerizing."

### Pull-Quote (locked)
"I was at exactly the place I'd been searching for years."

### Hardware Founder-Voiced Intro (locked opening line)
"This didn't start as a product. It started as a problem."

Expansion (from COPY.md Hardware Origin section):
"A table full of Eurorack modules, an XY joystick, and a trumpet. It worked — beautifully — but it was never something anyone else could just pick up and play."

---

## Asset Inventory

### Files to Copy Before HTML Work

| Source (confirmed exists) | Destination | Plan |
|---------------------------|-------------|------|
| `C:/Users/Milosh Xhavid/Desktop/Milosh Project/BA Konzert/Bild 2.JPG` | `assets/founder-dimitri.jpg` | 02-01 |
| `C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/WhatsApp Image 2026-01-17 at 00.04.34.jpeg` | `assets/hw-origin-performance.jpg` | 02-02 |
| `C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/Dimeola Schalltplan.jpg` | `assets/hw-origin-schematic.jpg` | 02-02 |

All three source files confirmed present via filesystem check on 2026-03-13.

### Current assets/ Directory
- `assets/arcade-chord-control.png` — already present, not touched by this phase

---

## Precise Insertion Points in index.html

| What | Line Reference | Action |
|------|---------------|--------|
| `#pageAbout` section HTML | After line 759 (`</div>` closing `#pageHome`) | Insert new `<div class="page" id="pageAbout">` block |
| About nav link (desktop) | Line 635 area — after Plugin `<li>` | Insert `<li><a href="#pageAbout" id="navAbout">About</a></li>` |
| About nav link (mobile) | Line 4001 area — after Plugin `<li>` | Insert `<li><a href="#pageAbout">About</a></li>` |
| `.about-section` CSS | Inside `<style>` block before `</style>` (line 621) | Append new CSS rules |
| Founder intro paragraph (hw) | Between line 772 and 774 (between `.hw-header` and `.hw-origin`) | Insert `.hw-founder-intro` div |
| hw-origin-performance img src | Line 777 | Replace broken path with `assets/hw-origin-performance.jpg` |
| hw-origin-schematic img src | Line 782 | Replace broken path with `assets/hw-origin-schematic.jpg` |

---

## State of the Art

| Old Approach | Current Approach | Impact |
|--------------|------------------|--------|
| Fixed `px` font sizes for big headings | `clamp(min, preferred, max)` | Scales smoothly across all viewport widths — use this |
| Separate mobile and desktop layouts | Single CSS grid collapsing at 900px breakpoint | Less code, already established in this codebase |

**Deprecated/outdated in this codebase:**
- Fixed pixel heights on images: all image containers use `aspect-ratio` or `object-fit` — follow this pattern for `.about-photo`

---

## Open Questions

1. **Photo crop — which part of Bild 2.JPG is most flattering?**
   - What we know: File exists, format is JPEG, no preview available in research phase
   - What's unclear: Whether `object-position: center top` is correct or if center/center works better for this specific photo
   - Recommendation: Use `object-position: center top` as default (works for most portrait shots); note in plan that implementer should visually confirm crop

2. **Nav link label — "About" vs "Story"?**
   - What we know: CONTEXT.md says "Add 'About' nav link" — this is locked
   - What's unclear: Nothing — "About" is the locked label
   - Recommendation: Use "About" verbatim

---

## Sources

### Primary (HIGH confidence)
- `index.html` (lines 1-4044) — direct inspection of all CSS patterns, nav structure, insertion points
- `COPY.md` — approved copy for all text content, confirmed on 2026-03-13
- `CONTEXT.md` — locked implementation decisions from user discussion
- Filesystem glob — confirmed all three source images present on local desktop

### Secondary (MEDIUM confidence)
- `PROJECT.md` — brand context, founder story background
- `STATE.md` — phase 01 completion status confirmed

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — no new dependencies, all patterns verified in live codebase
- Architecture: HIGH — all patterns traced to specific line numbers in index.html
- Copy: HIGH — locked verbatim in CONTEXT.md and confirmed in COPY.md
- Asset paths: HIGH — source files confirmed present via glob scan
- Pitfalls: HIGH — Windows path-with-spaces and image copy issues are well-known and pre-empted

**Research date:** 2026-03-13
**Valid until:** Stable — single static HTML file with no external dependencies or API versioning concerns
