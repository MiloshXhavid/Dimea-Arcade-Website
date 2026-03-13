# Phase 01: Fix, Polish & Mobile — Plan

**Milestone:** v1.0 — Launch-Ready Site
**Status:** In progress

---

## Sub-plan 01-01: Console Errors + Audio Verification

**Goal:** Zero JS console errors. Confirm audio fix works across browsers.

**Already fixed:**
- `audioCtx.resume()` now awaits before `triggerChord` fires (async race condition)
- `miniDisp` null crash fixed with `.filter(Boolean)` in `triggerChord`

**Tasks:**
1. Open `index.html` in Chrome — open DevTools Console (F12)
2. Check for any remaining red errors — document each one
3. Known missing element: `id="miniChord"` — already handled with null guard. Confirm no other missing element IDs are referenced in JS
4. Grep all `document.getElementById(...)` calls, verify each ID exists in HTML
5. Check for any `404` errors in Network tab (fonts, external resources)
6. Verify audio works: click joystick, should hear chord immediately on first click

**Done when:** Console shows zero errors, joystick plays audio on first click.

---

## Sub-plan 01-02: Mobile Responsive Layout

**Goal:** Full mobile support — all elements visible, nothing hidden. Working nav overlay, stacked hero, touch joystick.

**File:** `C:/Users/Milosh Xhavid/dimea-website/index.html`

### 1. Fix mobile nav — add `[ MENU ]` overlay toggle

**Remove** from the mobile breakpoint (around line 543-544):
```css
.nav-links { display:none; }
```

**Add** a menu toggle button to the nav HTML (after `.nav-cta` button):
```html
<button class="nav-menu-toggle" id="navMenuToggle" aria-label="Menu">[ MENU ]</button>
```

**Add** a full-screen overlay element (inside `<body>`, before closing `</body>`):
```html
<div class="nav-overlay" id="navOverlay">
  <ul class="nav-overlay-links">
    <li><a href="#pageHome">Home</a></li>
    <li><a href="#plugins">Plugin</a></li>
    <li><a href="#pageHardware">Hardware</a></li>
    <li><a href="#pageImpressions">Impressions</a></li>
    <li><a href="#pageShop">Shop</a></li>
    <li><a href="#pageApp">App</a></li>
  </ul>
</div>
```

**Add CSS** for toggle button and overlay:
```css
.nav-menu-toggle {
  display: none;
  font-family: 'DM Mono', monospace;
  font-size: 10px;
  letter-spacing: .2em;
  text-transform: uppercase;
  color: var(--teal);
  background: none;
  border: 1px solid rgba(42,232,196,.3);
  padding: 8px 16px;
  cursor: pointer;
}
.nav-overlay {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 800;
  background: rgba(5,8,16,0.97);
  backdrop-filter: blur(18px);
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.nav-overlay.open { display: flex; }
.nav-overlay-links {
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 32px;
}
.nav-overlay-links a {
  font-family: 'Syne', sans-serif;
  font-size: 32px;
  font-weight: 800;
  letter-spacing: .05em;
  color: var(--text);
  text-decoration: none;
  transition: color .2s;
}
.nav-overlay-links a:hover { color: var(--teal); }

@media (max-width: 900px) {
  .nav-menu-toggle { display: block; }
  .nav-links { display: none; }
  .nav-cta { display: none; }
}
```

**Add JS** to wire toggle (before closing `</script>`):
```js
const navToggle = document.getElementById('navMenuToggle');
const navOverlay = document.getElementById('navOverlay');
navToggle.addEventListener('click', () => {
  const open = navOverlay.classList.toggle('open');
  navToggle.textContent = open ? '[ CLOSE ]' : '[ MENU ]';
});
navOverlay.querySelectorAll('a').forEach(a => {
  a.addEventListener('click', () => {
    navOverlay.classList.remove('open');
    navToggle.textContent = '[ MENU ]';
  });
});
```

---

### 2. Fix mobile hero — show joystick, stack vertically

**Change** the mobile breakpoint (around line 543):
```css
/* BEFORE */
.hero { grid-template-columns: 1fr; }
.hero-right { display: none; }
.hero-left { padding: 120px 28px 80px; }

/* AFTER */
.hero { grid-template-columns: 1fr; }
.hero-right { display: flex; justify-content: center; padding: 40px 28px 80px; }
.hero-left { padding: 120px 28px 40px; }
.xy-world { width: 280px; height: 280px; }
```

---

### 3. Fix cursor for touch devices

**Add** after existing cursor CSS (around line 26-27):
```css
@media (pointer: coarse) {
  body { cursor: auto; }
  .cursor, .cursor-ring { display: none; }
  a, button, [role="button"] { cursor: pointer; }
  .xy-pad { cursor: crosshair; }
}
```

---

### 4. Verify touch joystick

Touch event listeners already exist (lines 2269–2271) and `getRelPos` already handles `e.touches[0]` (line 2128). Test on a real mobile device or Chrome DevTools mobile emulator:
- Open DevTools → Toggle device toolbar → Select a phone size
- Tap and drag the joystick — chord should play and dot should follow finger

**Done when:** On 375px wide viewport — nav shows `[ MENU ]`, tapping it opens full-screen overlay with all links. Hero shows text above, joystick below. Joystick responds to touch. No broken layouts in any section.

---

## Sub-plan 01-03: Media Embeds

**Goal:** Plugin screenshot and demo video embedded in plugin section.

**File:** `C:/Users/Milosh Xhavid/dimea-website/index.html`

### 1. Plugin screenshot

**Prerequisite:** Copy and rename file:
- Source: `C:/Users/Milosh Xhavid/Desktop/DIMEA Webseite/Screenshot 2026-03-06 022439.png`
- Destination: `C:/Users/Milosh Xhavid/dimea-website/assets/arcade-chord-control.png`
- Create `assets/` folder in repo if it doesn't exist

**Find** the plugin photo frame in HTML (around line 699):
```html
<div class="plugin-ui-frame plugin-ui-frame--photo">
```

**Replace** the CSS mock UI content inside that frame with:
```html
<div class="plugin-ui-frame plugin-ui-frame--photo">
  <div class="plugin-ui-frame-bar">DIMEA CHORD JOYSTICK MK2<div class="ui-close"></div></div>
  <img src="assets/arcade-chord-control.png" alt="Arcade Chord Control plugin screenshot" style="width:100%;display:block;">
</div>
```

Add CSS for the frame bar if not already present:
```css
.plugin-ui-frame-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'DM Mono', monospace;
  font-size: 8px;
  letter-spacing: .25em;
  text-transform: uppercase;
  color: var(--text-faint);
  padding: 8px 12px;
  background: rgba(0,0,0,.4);
  border-bottom: 1px solid var(--border);
}
```

---

### 2. Demo video embed

**Prerequisite (user action required):**
- Upload `IMG_7305.mov` to YouTube or Vimeo
- Get the embed URL (YouTube: `https://www.youtube.com/embed/VIDEO_ID`, Vimeo: `https://player.vimeo.com/video/VIDEO_ID`)
- Replace `VIDEO_URL` placeholder below with real URL

**Add** below the plugin UI frame (after screenshot, before feature list):
```html
<div class="plugin-video-wrap" style="margin-top:32px;position:relative;padding-bottom:56.25%;height:0;overflow:hidden;border:1px solid var(--border-mid);">
  <iframe
    src="VIDEO_URL"
    style="position:absolute;top:0;left:0;width:100%;height:100%;border:none;"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen
    loading="lazy">
  </iframe>
</div>
```

**Done when:** Plugin section shows real screenshot inside branded frame. Video iframe is present and plays on click. Both display correctly on mobile.

---

## Execution order

1. **01-01** — Verify audio fix in browser, clear any remaining console errors
2. **01-02** — Mobile layout (can do without user input)
3. **01-03** — Screenshot first (can do now), video after user uploads and provides URL
