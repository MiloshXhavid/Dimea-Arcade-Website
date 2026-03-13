# Phase 02: About / Story / Vision — Context

**Gathered:** 2026-03-13
**Status:** Ready for planning

<domain>
## Phase Boundary

Add the founder story, manifesto, and brand vision as a dedicated section (`#pageAbout`) in the site's scroll flow. This is the emotional core that sells the person, not just the product.

Also enriches the hardware page: adds a founder-voiced intro paragraph above the existing origin strip and fixes two broken image paths.

No new capabilities beyond the ROADMAP scope — no CMS, no blog, no comments.

</domain>

<decisions>
## Implementation Decisions

### Section Placement
- New standalone scroll-snap page-section: `<div class="page" id="pageAbout">`
- Positioned between `#pageHome` and `#pageHardware` in the DOM
- Add "About" nav link to both the desktop nav (`.nav-links`) and the mobile overlay (`.nav-overlay-links`)

### Manifesto vs Bio Structure
- **Manifesto leads** — section opens with the big quote, then bio follows:
  1. Full-width giant manifesto quote: *"Most musicians spend years learning theory just to play a chord. We think that's backwards."*
  2. Brief mission expansion (1-2 sentences: "remove the barrier between people and harmony")
  3. Founder bio (3-4 sentences) + portrait photo alongside

### Visual Treatment
- Manifesto quote: Syne font-weight 800, ~5-7vw, white text on navy-deep background
- Teal (#2ae8c4) accent on one key word (e.g. "backwards" or "harmony")
- Section background: `var(--navy-deep)` matching hardware/impressions pages
- Bio section: two-column layout — text left, photo right (stacks vertically on mobile)

### Founder Bio Copy (use this verbatim, short version)
"Dimitri Dähler is a jazz trumpet player, producer, and sound engineer trained at the Hochschule Luzern. The origin of Dimea Arcade was a single frustration: 'I wanted chords to improvise over on my trumpet. People told me modular can't do chords. So I built one that could.' That modular rig became his masterproject performance at HSLU. The audience called it mesmerizing."

Pull-quote below bio: *"I was at exactly the place I'd been searching for years."*

### Founder Photo
- Use real photo: `Bild 2.JPG` from BA Konzert folder
- Source: `C:/Users/Milosh Xhavid/Desktop/Milosh Project/BA Konzert/Bild 2.JPG`
- Copy to: `assets/founder-dimitri.jpg`
- Treatment: portrait crop (3:4 or 4:5 aspect ratio), `object-fit: cover`
- Subtle teal border or no border — Claude's discretion based on what looks good

### Claude's Discretion
- Exact aspect ratio of photo container (3:4 vs 4:5)
- Whether manifesto quote uses one-word teal accent or a phrase
- Spacing and padding values within the section
- Whether the bio text and photo are on equal-width columns or asymmetric

---

### Hardware Origin Story (02-02)

#### Founder-Voiced Intro
- Add a short paragraph ABOVE the existing `.hw-origin` strip (between `.hw-header` and `.hw-origin`):
  > *"This didn't start as a product. It started as a problem."*
  > Brief expansion (2-3 sentences from PROJECT.md founder story, setting up the modular years).
- Keep the existing 3-step strip (Modular → Plugin → Hardware) exactly as-is — it's well-written
- Keep existing captions as-is

#### Fix Broken Image Paths
Both images currently use broken `/mnt/user-data/uploads/` paths. Fix both:

| Current broken path | Desktop source | New asset path |
|---|---|---|
| `/mnt/user-data/uploads/WhatsApp_Image_2026-01-17_at_00_04_34.jpeg` | `C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/WhatsApp Image 2026-01-17 at 00.04.34.jpeg` | `assets/hw-origin-performance.jpg` |
| `/mnt/user-data/uploads/Dimeola_Schalltplan.jpg` | `C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/Dimeola Schalltplan.jpg` | `assets/hw-origin-schematic.jpg` |

Both images stay in their current positions in the HTML — just update the `src` attributes.

</decisions>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.section-label` class: teal DM Mono label with left bar — reuse for "About" section label
- `.hw-vision-title` / `.hw-vision-body`: established heading + body pattern for content sections
- `.hw-origin`: existing origin strip layout — don't replace, just prepend founder intro above it
- CSS variables: `--navy-deep`, `--navy-mid`, `--teal`, `--text`, `--text-dim`, `--border` — all available

### Established Patterns
- Sections use: `padding: 120-130px 52px` (desktop), `100px 28px 80px` (mobile `@media(max-width:900px)`)
- Page sections: `<div class="page" id="pageX">` → `<section class="x-section" id="x">`
- Section label: `<div class="section-label">Label Text</div>` above every heading
- Two-column layouts use CSS Grid: `grid-template-columns: 1fr 1fr` or asymmetric ratios
- `clamp()` used for responsive font sizes throughout

### Integration Points
- Nav update: `.nav-links` ul (desktop, around line 634) + `.nav-overlay-links` ul (mobile overlay, around line 3999)
- DOM insertion: After closing `</div>` of `#pageHome` (~line 760), before `<!-- HARDWARE PAGE -->` comment
- Hardware page insertion: After `.hw-header` div (~line 773), before `.hw-origin` div (~line 774)

</code_context>

<specifics>
## Specific Ideas

- The manifesto quote is locked: *"Harmony shouldn't live behind a theory wall. It should be something you feel from the first note."* — use verbatim. Teal accent on "theory wall."
- Founder photo: `WhatsApp Image 2026-01-17 at 00.04.34.jpeg` — explicitly chosen
- Founder-voiced hardware intro: start with *"This didn't start as a product. It started as a problem."* — use verbatim from PROJECT.md
- Pull-quote: *"I was at exactly the place I'd been searching for years."* — use verbatim

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 02-about-story-vision*
*Context gathered: 2026-03-13*
