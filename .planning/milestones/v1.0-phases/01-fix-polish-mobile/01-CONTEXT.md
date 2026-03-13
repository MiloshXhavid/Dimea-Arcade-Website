# Phase 01: Fix, Polish & Mobile — Context

**Milestone:** v1.0 — Launch-Ready Site
**Goal:** Take index.html from prototype to production-ready. Zero console errors, working audio, full mobile layout, media embedded.

---

## Current state

- `index.html` exists (single-file site, ~2400 lines)
- Audio bug fixed: `audioCtx.resume()` now awaits before chord fires (was async race condition)
- Mobile nav: `nav-links` hidden via `display:none` — no toggle exists yet
- Mobile hero: `hero-right` (joystick) set to `display:none` — needs to show
- Plugin screenshot: `plugin-ui-frame--photo` frame exists, no `<img>` inside
- Demo video: not embedded anywhere
- Custom cursor: `cursor:none` global — needs mobile override

## Files

- Main file: `C:\Users\Milosh Xhavid\dimea-website\index.html`
- Demo video: `C:\Users\Milosh Xhavid\Desktop\DIMEA Webseite\IMG_7305.mov` → host on YouTube or Vimeo
- Plugin screenshot: `C:\Users\Milosh Xhavid\Desktop\DIMEA Webseite\Screenshot 2026-03-06 022439.png` → rename to `arcade-chord-control.png`, copy into repo

---

## Decisions

### Mobile Nav

- All 5 nav sections (Home, Plugin, Hardware, Impressions, Shop, App) must be accessible on mobile
- Nav CTA button ("Explore Free") stays visible as-is — no change
- Toggle button: `[ MENU ]` / `[ CLOSE ]` text in DM Mono, positioned top-right
- Open state: full-screen dark overlay, nav links centered, brand-styled (navy/teal)
- No hamburger icon — text toggle only, consistent with existing DM Mono label style

### Mobile Layout

- Full mobile support required — all elements visible, nothing hidden
- Remove `hero-right { display:none }` — joystick must show on mobile
- Remove `nav-links { display:none }` — replaced by overlay toggle system
- `cursor:none` must be overridden for touch devices via `@media (pointer:coarse)` — restore `cursor:pointer` on interactive elements
- Hero layout: stack vertically on mobile (left content above, joystick below)

### XY Joystick — Touch Support

- Add `touchstart`, `touchmove`, `touchend` event listeners alongside existing mouse events
- Already partially done: listeners exist at lines 2269–2271 but position extraction (`getRelPos`) may need touch coordinate handling verified
- Touch events use `e.touches[0]` for coordinates — confirm `getRelPos` handles this

### Demo Video

- Host on YouTube or Vimeo (user's choice of platform)
- Embed via `<iframe>` in plugin section
- Position: below the plugin screenshot
- Behaviour: play-on-click only (no autoplay, no loop)
- No label — do not call attention to it being a rough demo

### Plugin Screenshot

- File: rename `Screenshot 2026-03-06 022439.png` → `arcade-chord-control.png`
- Copy into repo at: `assets/arcade-chord-control.png`
- Replaces the CSS-drawn fake UI (`plugin-ui-frame` with mock knobs/sliders)
- Display: wrapped in the existing `plugin-ui-frame` chrome (dark header bar, "DIMEA CHORD JOYSTICK MK2" label, magenta close dot)
- No platform compatibility note — plugin is cross-platform (VST3/AU/AAX, Windows + Mac)

---

## Code context

| Element | Location | Notes |
|---------|----------|-------|
| `initAudio()` / `activate()` | ~line 1857 / 2197 | Audio race condition fixed |
| `audioCtx.resume()` fix | line 2199–2223 | Now awaits before `triggerChord` |
| Mobile nav hide | line 544 | `.nav-links{display:none}` — replace with overlay system |
| Mobile hero hide | line 543 | `.hero-right{display:none}` — remove, stack vertically |
| Touch event listeners | lines 2269–2271 | Exist, verify `getRelPos` handles `e.touches[0]` |
| Plugin UI frame | ~line 168–186 | CSS-drawn mock — replace content with real screenshot |
| Plugin photo frame | ~line 699 | `plugin-ui-frame--photo` — `<img>` goes here |
| Game overlay | ~line 2382 | Separate audio context (`actx`) — not related to joystick |

---

## Sub-plans

- **01-01:** Diagnose + fix remaining console errors; verify audio fix works across browsers
- **01-02:** Mobile responsive layout — nav overlay, hero stack, joystick touch support, cursor fix
- **01-03:** Media embeds — plugin screenshot (rename + copy + frame) + video iframe (after upload)
