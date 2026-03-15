# Phase 10: Site Reframe — Plugin First - Research

**Researched:** 2026-03-15
**Domain:** Flat HTML content surgery — hero, shop, nav, section labelling
**Confidence:** HIGH

## Summary

This phase is a content and markup surgery on a single flat HTML file (`index.html`, ~4 074 lines). There is no framework, no build step, no component system. Every change is a direct edit to the HTML text. The risk profile is low technically but high for regression — a careless find-replace could break IDs that JavaScript depends on (scroll-spy, `showPage()`, mobile nav close), and removing cards from the shop grid may leave orphaned CSS that causes layout shift.

The plugin is already the secondary CTA in the hero (`btn-secondary`) and the preorder hardware button is the primary (`btn-primary`). Swapping that hierarchy is the single most impactful change. The hardware section already uses an "In Development" tone in the status bar text; the main work is stripping the price out of the status tag and removing `~CHF 499` from the spec list and the shop card. The App section already shows an "IN DEVELOPMENT" amber badge inline — no structural change needed there, only tone-check and removal of any purchase CTA if one exists (none found in audit).

The shop grid has three cards: plugin (available), hardware preorder, and bundle dim-card. Both the hardware and bundle cards must be removed, leaving a single-card layout. The shop description copy `"Plugin licenses, hardware pre-orders, and future drops — in one place."` will need updating to reflect plugin-only. A "More coming" line replaces the two removed cards.

**Primary recommendation:** Execute as four discrete, testable edits — (1) Hero button swap, (2) Hardware section price/CTA removal, (3) Shop collapse to one card, (4) Nav + section divider label rename — in that order, committing after each is verified.

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Hero Section**
- Remove "Preorder Hardware — CHF 499" primary button entirely
- "Get Plugin — CHF 49" becomes the primary CTA (btn-primary)
- "Explore Free →" stays as the secondary / ghost CTA
- Hero subtitle should be audited — remove or soften any hardware sales language
- Hero communicates: plugin is available now

**Hardware Section (pageHardware)**
- Keep the section — it tells the brand story and shows vision
- Remove all pricing (CHF 499, "~CHF 499") from status bar and any other elements
- Remove any deposit / preorder CTAs or language
- Waitlist form stays (Phase 09 wires it to Mailchimp — that work is separate)
- Reframe section tone: "this is where we're headed" not "buy this"
- The hardware section label/status can say something like "In Development" or "Coming"

**App Section (pageApp)**
- Same treatment as hardware — vision framing, no price, no purchase CTA
- "Coming Soon" or "In Development" badge/label
- No CTA beyond awareness / waitlist if applicable

**Shop Section (pageShop)**
- Collapse from 3-card grid to plugin-only
- Remove DIMEOLA preorder card
- Remove Bundle card
- Add a small "More coming" note below the plugin card (not another card — just a line or small text element)
- Shop is now a plugin sales section, not a multi-product catalog

**Navigation**
- "Hardware" nav link → rename label to "Vision" (keeps the anchor, points to hardware section)
- "App" nav link → rename label to "Vision" or fold into the same concept
  - If both Hardware and App sections exist and are both "vision" sections, consider whether two separate nav links both called "Vision" is confusing — Claude's discretion on naming (e.g. "Roadmap", "Future", or keep distinct names)
- Section divider labels should be updated to match

### Claude's Discretion
- Exact wording for "In Development" badges / labels
- Whether to add a one-liner in the hardware section header like "Not for sale yet — follow the build"
- How to handle the mobile nav (same label renames apply)
- Footer navigation link labels

### Deferred Ideas (OUT OF SCOPE)
- Stripe hardware deposit → deferred until Kickstarter-ready
- Kickstarter campaign → Phase 19 (v5.0 milestone)
- Phase 10 original scope (Stripe Payment Link wiring) is cancelled for now
- Crypto checkout for hardware → Phase 11 (still applies to plugin, hardware deposit deferred)
</user_constraints>

---

## Standard Stack

### Core
| Item | Detail | Notes |
|------|--------|-------|
| File | `index.html` (single file, ~4 074 lines) | All edits happen here |
| Language | Vanilla HTML + CSS + JS | No framework, no build step |
| Hosting | Vercel static | No server-side rendering |
| CSS approach | Inline `<style>` block in `<head>` | All styles in one file |
| JS approach | Inline `<script>` blocks at end of body | No external modules |

### No Installation Required

This phase requires zero new dependencies. All edits are text changes to `index.html`.

## Architecture Patterns

### Recommended Edit Order

Execute as four independent, verifiable units:

```
Wave 1: Hero CTA swap
Wave 2: Hardware section — price removal + tone
Wave 3: Shop — collapse to single card + "More coming"
Wave 4: Nav + section dividers + mobile nav + footer nav
```

Committing after each wave means a regression can be bisected immediately.

### Page / Section Structure (current)

```
index.html
├── <head>   — inline CSS (~900 lines)
├── <nav>    — desktop nav (.nav-links) + mobile toggle
├── #pageHome / .hero   — hero section
├── section-divider "02 About"
├── #pageAbout
├── section-divider "03 Hardware"
├── #pageHardware       — hw-status-bar, specs, origin, roadmap, waitlist
├── section-divider "04 Impressions"
├── #pageImpressions
├── section-divider "05 Shop"
├── #pageShop           — 3-card .shop-grid
├── section-divider "06 App"
├── #pageApp
├── <footer>            — footer nav links
├── <!-- MOBILE NAV OVERLAY -->   — separate <div> after footer
└── <script> blocks     — scroll-spy, mobile nav, etc.
```

### Scroll-Spy JavaScript Dependency

The scroll-spy array at line ~1836 maps page IDs to nav link IDs. It does NOT include `pageApp` or `pageShop` — only `pageHome`, `pageAbout`, `pageHardware`, `pageImpressions`. This means renaming nav link text is safe as long as element `id` attributes are not changed.

```javascript
// Current scroll-spy array (lines ~1836–1840)
[
  {id:'pageHome',        nav:'navPlugins'},
  {id:'pageAbout',       nav:'navAbout'},
  {id:'pageHardware',    nav:'navHardware'},
  {id:'pageImpressions', nav:'navImpressions'},
]
```

**Critical:** `id="navHardware"` is referenced by JS. The `id` must remain `navHardware`. Only the visible text inside the `<a>` tag changes.

### Nav Link Naming Recommendation (Claude's Discretion)

Two sections labelled identically ("Vision" / "Vision") in the nav would be confusing. Recommended approach:

| Nav item | Current label | Recommended label | Rationale |
|----------|--------------|------------------|-----------|
| `#pageHardware` | Hardware | Vision | Hardware IS the primary vision product |
| `#pageApp` | App | Roadmap | App is downstream of the vision; "Roadmap" captures both app + future plans |

This avoids duplication, keeps each link semantically distinct, and matches the brand tone (exploring a direction, not selling a product).

Alternatively: rename Hardware → "Vision" and remove App from the desktop nav entirely (it is already absent from the scroll-spy array). This is a lighter change but loses the App section discoverability. Not recommended — the App section has real content.

### Shop Card CSS Classes (to audit before deletion)

The three cards use distinct classes:
- `.shop-card--available` — plugin card (KEEP)
- `.shop-card--preorder` — hardware card (REMOVE)
- `.shop-card--dim` — bundle card (REMOVE)

CSS for `.shop-card--preorder` and `.shop-card--dim`, `.shop-badge--soon`, `.shop-badge--tbd` will become dead code after card removal. These can be left in place (harmless) or pruned. Pruning is optional — leaving dead CSS does not cause visible regressions and avoids touching the style block unnecessarily.

The `.shop-grid` currently uses an implicit 3-column layout. With one card remaining, the grid will reflow to a single centred column (or whatever `grid-template-columns` specifies). This should be audited to ensure the single card does not stretch full-width in an ugly way. A `max-width` constraint on `.shop-grid` or the single card may be appropriate.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Text find-replace | Shell sed / regex replace | Direct manual Read + Edit per element | The file has many `CHF 499` occurrences with different contexts — each must be audited individually |
| Shop grid single-column | New grid CSS | Verify existing `.shop-grid` CSS reflows cleanly | Grid likely already handles 1-column via auto-fill or media queries |

**Key insight:** This phase is a precision content edit, not an engineering task. The danger is collateral damage from bulk edits, not from missing libraries.

## Common Pitfalls

### Pitfall 1: Breaking the navHardware scroll-spy ID
**What goes wrong:** Renaming the `id` on the `<a>` tag from `navHardware` to something else breaks the scroll-spy JS that calls `document.getElementById('navHardware')`.
**Why it happens:** It looks like a rename when you're editing labels, but the `id` attribute and the text content are different things.
**How to avoid:** Change only the text inside the `<a>` tag. Leave `id="navHardware"` untouched.
**Warning signs:** After deploy, active nav link highlighting stops working when scrolled to the hardware section.

### Pitfall 2: CHF 499 in the roadmap timeline copy
**What goes wrong:** `~CHF 499` appears in three distinct locations — `hw-status-bar` (line 1191), the spec list `Target price` row (line 1334), and the roadmap timeline step description (line 1380: "Kickstarter campaign targeting ~CHF 499"). All three must be addressed.
**Why it happens:** A narrow search for "CHF 499" might miss the roadmap line if the string is phrased differently, or an editor might remove it from the status bar but forget the spec table.
**How to avoid:** Grep for `499` across the whole file before and after editing to confirm zero instances remain.
**Warning signs:** Price still visible in the "Beta Units + Crowdfunding" roadmap timeline card.

### Pitfall 3: Shop grid going full-width with one card
**What goes wrong:** The `.shop-grid` was designed for three cards. With one card, it may render as a single column spanning the full container width, which looks strange for a single product card.
**Why it happens:** CSS Grid `grid-template-columns` with `repeat(auto-fill, minmax(...))` or a fixed 3-column definition will reflow — but depending on the implementation the single card may be 100% wide.
**How to avoid:** After removing the two cards, inspect the layout. If the card stretches, add `max-width` to `.shop-grid` or the card. Check existing CSS at the `.shop-grid` rule.
**Warning signs:** Plugin card renders as a wide landscape slab instead of a focused product card.

### Pitfall 4: Hero `btn-secondary` and `btn-ghost-text` class semantics
**What goes wrong:** The current hero has three CTAs: `btn-primary` (preorder hardware), `btn-secondary` (get plugin), `btn-ghost-text` (Explore Free). The decision is to make the plugin the `btn-primary` and keep Explore Free as ghost. The middle-tier `btn-secondary` class may not suit the "Explore Free" button if it needs to stay ghost.
**Why it happens:** After removing the hardware button, "Get Plugin" becomes primary. "Explore Free" should remain low-emphasis (ghost). No second button is needed, so the class hierarchy collapses from 3 levels to 2.
**How to avoid:** The spec says "Explore Free → stays as the secondary / ghost CTA." In context this means it retains its `btn-ghost-text` class. The `btn-secondary` class can be replaced with `btn-primary` on the plugin link. No new class needed.
**Warning signs:** "Explore Free" button appears with a coloured border/fill when it should be minimal.

### Pitfall 5: Mobile nav overlay (lines ~4001–4014) is a separate duplicate
**What goes wrong:** The mobile nav overlay is NOT inside the `<nav>` element — it is a separate `<div id="navOverlay">` at the very bottom of the DOM (after the footer). It has its own list of `<li><a>` links. Renaming the desktop nav labels without updating the overlay leaves the mobile nav showing the old names.
**Why it happens:** The mobile nav is visually connected to the desktop nav but lives in a completely different part of the HTML. Easy to miss.
**How to avoid:** Always update both nav locations in the same edit. The mobile overlay links at lines ~4004–4009 mirror the desktop `<ul class="nav-links">` at lines ~902–909.

### Pitfall 6: Footer nav links are a third nav location
**What goes wrong:** The footer at lines ~1742–1748 has a third copy of the navigation links. "Hardware" and "App" appear there too.
**Why it happens:** Three separate nav implementations: desktop nav, mobile overlay, footer nav.
**How to avoid:** The footer links use plain text labels (no IDs). Update all three when renaming nav labels.

### Pitfall 7: Shop description copy still references hardware pre-orders
**What goes wrong:** Line 1520: `"Plugin licenses, hardware pre-orders, and future drops — in one place."` — this copy becomes false when the hardware card is removed.
**Why it happens:** Header copy and card content are edited separately.
**How to avoid:** Update the `.shop-desc` text as part of the shop collapse task.

### Pitfall 8: Waitlist form onclick in the shop hardware card
**What goes wrong:** The hardware shop card's "Join Waitlist →" button (line 1608) has an inline `onclick` that scrolls to `#pageHardware` and focuses the email input. When this card is removed, the onclick is removed with it — no orphan JS.
**Why it happens:** N/A — removal is clean.
**How to avoid:** Just confirm the card's onclick does not register any global event listener (it doesn't — it's a pure inline scroll call). Safe to delete.

## Code Examples

### Current hero-actions block (lines 930–934)
```html
<!-- CURRENT — three CTAs, hardware is primary -->
<div class="hero-actions">
  <a href="#" class="btn-primary">Preorder Hardware — CHF 499 <svg ...></svg></a>
  <a href="#plugins" class="btn-secondary">Get Plugin — CHF 49</a>
  <button class="btn-ghost-text" id="exploreFreeHero" style="...">Explore Free →</button>
</div>
```

### Target hero-actions block (after phase 10)
```html
<!-- TARGET — plugin is primary, hardware button removed -->
<div class="hero-actions">
  <a href="#pageShop" class="btn-primary">Get Plugin — CHF 49 <svg ...></svg></a>
  <button class="btn-ghost-text" id="exploreFreeHero" style="...">Explore Free →</button>
</div>
```

Notes:
- The `href` on the plugin button should point to `#pageShop` (the purchase flow lives there) or to the LemonSqueezy overlay (Phase 08). Confirm with Phase 08 implementation — if the shop card has the checkout flow, `#pageShop` is correct.
- Arrow SVG from the removed hardware button can be reused on the plugin button for visual consistency.

### Current hw-status-bar (lines 1187–1192)
```html
<div class="hw-status-bar">
  <div class="hw-status-dot"></div>
  <span class="hw-status-text">CONCEPT &amp; PROTOTYPE STAGE</span>
  <div class="hw-status-line"></div>
  <span class="hw-status-tag">Daisy Seed · Pure Data · ~CHF 499</span>
</div>
```

### Target hw-status-bar
```html
<div class="hw-status-bar">
  <div class="hw-status-dot"></div>
  <span class="hw-status-text">IN DEVELOPMENT</span>
  <div class="hw-status-line"></div>
  <span class="hw-status-tag">Daisy Seed · Pure Data · Concept & Prototype</span>
</div>
```

### Current desktop nav hardware link (line 905)
```html
<li><a href="#pageHardware" id="navHardware">Hardware</a></li>
```

### Target desktop nav hardware link
```html
<li><a href="#pageHardware" id="navHardware">Vision</a></li>
```

### Current section divider "03 Hardware" (line 1126)
```html
<div class="section-divider"><span class="section-divider-num">03</span><span class="section-divider-name">Hardware</span><div class="section-divider-line"></div></div>
```

### Target section divider
```html
<div class="section-divider"><span class="section-divider-num">03</span><span class="section-divider-name">Vision</span><div class="section-divider-line"></div></div>
```

### "More coming" note placement (after shop collapse)
```html
<!-- after closing </div> of the single plugin shop-card, inside .shop-grid -->
<p class="shop-more-coming">More products in development — hardware and app coming in 2026.</p>
```

A CSS rule for `.shop-more-coming` is needed (or an inline style). Suggested:
```css
.shop-more-coming {
  font-family: 'DM Mono', monospace;
  font-size: 11px;
  letter-spacing: .15em;
  text-transform: uppercase;
  color: var(--text-faint);
  text-align: center;
  padding: 20px 0 0;
  grid-column: 1 / -1;
}
```
`grid-column: 1 / -1` ensures the note spans the full grid width regardless of column count.

## Exact Locations of All CHF 499 Instances

Verified by file audit:

| Line | Context | Element | Action |
|------|---------|---------|--------|
| 931 | Hero primary button | `btn-primary` text | Remove entire button |
| 1191 | Hardware status bar | `.hw-status-tag` | Replace with tech stack only, no price |
| 1334 | Hardware spec list | `Target price` row | Remove entire `<li>` |
| 1380 | Roadmap timeline | "Kickstarter campaign targeting ~CHF 499" | Replace with "Kickstarter campaign. Early backers get direct input on the final spec." |
| 1606 | Shop hardware card | `.shop-price` | Remove entire hardware card |

## Exact Locations of All Nav / Label Touch Points

| Location | Line approx. | Current text | Action |
|----------|-------------|--------------|--------|
| Desktop nav `<ul class="nav-links">` | 905 | `Hardware` | → `Vision` |
| Desktop nav `<ul class="nav-links">` | 908 | `App` | → `Roadmap` |
| Section divider "03 Hardware" | 1126 | `Hardware` | → `Vision` |
| Section divider "06 App" | 1640 | `App` | → `Roadmap` |
| Hardware section `.section-label` | 1135 | `Hardware` | → `Vision` (optional — matches divider) |
| App section `.section-label` | 1647 | `App` | → `Roadmap` (optional — matches divider) |
| Mobile nav overlay | ~4006 | `Hardware` | → `Vision` |
| Mobile nav overlay | ~4009 | `App` | → `Roadmap` |
| Footer nav | ~1744 | `Hardware` | → `Vision` |
| Footer nav | ~1747 | `App` | → `Roadmap` |

## Open Questions

1. **Hero subtitle audit — does "The Arcade Chord Control turns your gamepad into a real-time harmony engine. Map scales, voices, and tension to physical sticks — then perform like never before." need changes?**
   - What we know: Current subtitle (line 929) is entirely plugin-focused. No hardware sales language present.
   - What's unclear: Whether the phrase "Map scales, voices, and tension to physical sticks" could be misread as referring to the hardware joysticks rather than a gamepad.
   - Recommendation: No change needed. The sentence already says "your gamepad" which anchors it to the plugin use case.

2. **Hero `href` for the plugin CTA button**
   - What we know: The current secondary plugin button has `href="#plugins"` (the plugin section). The shop card has the actual buy flow.
   - What's unclear: Phase 08 (LemonSqueezy) may have wired the button to an overlay trigger rather than a scroll link. If Phase 08 is live, the `href` or `onclick` should match the Phase 08 pattern.
   - Recommendation: Use `href="#pageShop"` as a safe default if Phase 08 is not yet live. If Phase 08 is live, mirror whatever the `Buy Now` button in the shop card uses.

3. **Shop grid single-card layout**
   - What we know: CSS Grid is used but the exact `grid-template-columns` value is not confirmed from the audit (the CSS is in the inline style block, not directly read for this rule).
   - What's unclear: Whether the grid will naturally centre one card or render it full-width.
   - Recommendation: Check `.shop-grid` CSS rule before implementing. If it uses a fixed 3-column or `repeat(3, 1fr)`, add `grid-template-columns: 1fr` override when only one card remains, or apply a `max-width` to the card directly. Investigate as part of Wave 3.

## Validation Architecture

> `workflow.nyquist_validation` not present in `.planning/config.json` — treating as enabled.

### Test Framework
| Property | Value |
|----------|-------|
| Framework | None (static HTML site, no test runner installed) |
| Config file | None |
| Quick run command | Manual browser inspection + grep verification |
| Full suite command | `grep -n "499\|Preorder\|preorder" index.html` should return 0 results |

### Phase Requirements → Test Map

No formal requirement IDs defined. Verification is by direct DOM inspection and text search.

| Behaviour | Test Type | Verification Command |
|-----------|-----------|----------------------|
| No CHF 499 price anywhere visible | Text search | `grep -n "499" index.html` — expect 0 matches |
| No "Preorder" text anywhere | Text search | `grep -ni "preorder" index.html` — expect 0 matches |
| Plugin CTA is `.btn-primary` in hero | Manual browser | Inspect hero — plugin button should be magenta/filled |
| Hardware section has no purchase CTA | Manual browser | Scroll to #pageHardware — no "Buy" / "Order" button visible |
| Shop shows only one card (plugin) | Manual browser | Scroll to #pageShop — single card visible, two cards gone |
| "More coming" note visible below shop card | Manual browser | Below plugin card, small text note present |
| Nav shows "Vision" not "Hardware" | Manual browser | Desktop + mobile nav inspection |
| Nav shows "Roadmap" not "App" | Manual browser | Desktop + mobile nav inspection |
| hw-waitlist form still present and functional | Manual browser | Scroll to bottom of hardware section — email input visible |

### Wave 0 Gaps
None — no test infrastructure exists or is needed for a static HTML content edit. Verification is manual + grep.

## Sources

### Primary (HIGH confidence)
- `index.html` direct file audit — line numbers and exact element content verified by Read tool
- `COPY.md` — approved brand copy, confirmed no hardware pricing in approved copy document
- `10-CONTEXT.md` — user decisions, locked and discretion areas

### Secondary (MEDIUM confidence)
- `.planning/STATE.md` — project history, confirms Phase 09 (waitlist) is separate concern

### Tertiary (LOW confidence)
- None

## Metadata

**Confidence breakdown:**
- Change locations: HIGH — all elements located by line number via direct file read
- CSS side-effects (shop grid): MEDIUM — exact `grid-template-columns` value not read; flagged as open question
- JS side-effects: HIGH — scroll-spy and mobile nav overlay fully audited, safe patterns confirmed

**Research date:** 2026-03-15
**Valid until:** Until `index.html` is next modified by another phase (Phase 08 or 09 changes may shift line numbers)
