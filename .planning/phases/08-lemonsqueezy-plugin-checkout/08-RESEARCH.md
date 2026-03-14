# Phase 08: LemonSqueezy Plugin Checkout — Research

**Researched:** 2026-03-14
**Domain:** LemonSqueezy Lemon.js overlay checkout — vanilla HTML/JS integration
**Confidence:** HIGH

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- Overlay checkout only — not redirect (keeps user on site, higher conversion)
- LemonSqueezy platform — not Gumroad, not Stripe direct
- Launch price CHF 49 / Regular CHF 79 — configured in LS dashboard, not hardcoded
- "Buy Now" button (existing, in plugin section) gets wired to LS overlay — no new buttons added
- "Try Free" button → demo download link (wire if link available, otherwise placeholder)
- Post-purchase: LS handles license key + download link delivery by email automatically
- No custom thank-you page needed this phase
- Site is flat HTML, vanilla JS only — no framework

### Claude's Discretion
- Exact JS snippet placement in index.html (before `</body>` preferred)
- Whether to show a "Powered by LemonSqueezy" note near button
- Button loading state while LS overlay initializes

### Deferred Ideas (OUT OF SCOPE)
- Mac installer upload (waiting for Mac signing ~2026-03-21)
- Crypto checkout option alongside LS (Phase 11)
- License key validation/activation on site
- Affiliate/referral tracking
</user_constraints>

---

## Summary

LemonSqueezy provides a first-party JavaScript library called **Lemon.js** (2.3 kB, hosted on their CDN) that intercepts clicks on designated elements and opens the checkout as a modal overlay instead of navigating away. No backend is required — the integration is entirely client-side for the overlay method.

The checkout URL format is `https://[STORE_SLUG].lemonsqueezy.com/checkout/buy/[VARIANT_UUID]`. This URL is obtained from the LS dashboard product page under "Share". It is a stable, reusable link — the variant UUID identifies the specific product variant (the Windows installer license).

The current `index.html` has three "Buy Now" touch-points that all need wiring: (1) the plugin section price block `<a href="#" class="btn-primary">Buy Now — CHF 49</a>` at line 1055, (2) the shop section `<button ... id="buyPluginBtn">Buy Now — CHF 49 →</button>` at line 1568 (currently opens a custom email-collection UI that must be replaced), and (3) the sticky CTA `<button>` at line 4054 which already delegates to `buyPluginBtn.click()`.

**Primary recommendation:** Add `<script src="https://app.lemonsqueezy.com/js/lemon.js" defer></script>` before `</body>`, convert the plugin-section anchor to `<a class="btn-primary lemonsqueezy-button" href="[LS_CHECKOUT_URL]">`, replace the shop-section button + email-form block with a plain `<a>` carrying `lemonsqueezy-button`, and update the sticky CTA's onclick to call `LemonSqueezy.Url.Open("[LS_CHECKOUT_URL]")` via JS.

---

## Standard Stack

### Core

| Library | Source | Purpose | Why Standard |
|---------|--------|---------|--------------|
| Lemon.js | `https://app.lemonsqueezy.com/js/lemon.js` | Intercept clicks, render overlay iframe | Official LS library — the only supported client-side overlay method |

### No npm install required
This is a CDN script tag. No build step, no package manager. Fully compatible with the flat HTML project structure.

**Include in HTML:**
```html
<script src="https://app.lemonsqueezy.com/js/lemon.js" defer></script>
```

Place immediately before `</body>` — consistent with the project's existing inline script placement pattern.

---

## Architecture Patterns

### How the Overlay Works

1. Lemon.js loads and calls `createLemonSqueezy()` automatically on DOMContentLoaded.
2. It attaches a click listener to every element with class `lemonsqueezy-button`.
3. On click, it reads the element's `href` (must be the LS checkout URL), opens an iframe overlay modal.
4. User completes purchase inside the iframe. LS sends license + download link by email automatically.
5. `Checkout.Success` event fires on the parent page if an event handler is set up.

### Pattern 1: Anchor Tag with `lemonsqueezy-button` Class (recommended for most buttons)

**What:** Any `<a>` with class `lemonsqueezy-button` and a valid LS checkout URL as its `href` will trigger the overlay on click.

**When to use:** Plugin section "Buy Now" button (currently an `<a>`), shop section "Buy Now" (must convert from `<button>` to `<a>`).

**Critical:** `lemonsqueezy-button` ONLY works reliably on `<a>` anchor tags, not on `<button>` elements. This is a known limitation confirmed by the LS docs ("copy and paste the HTML embed code to add a 'Buy' link") and multiple community sources.

```html
<!-- Source: https://docs.lemonsqueezy.com/guides/developer-guide/lemonjs -->
<a href="https://[STORE_SLUG].lemonsqueezy.com/checkout/buy/[VARIANT_UUID]"
   class="btn-primary lemonsqueezy-button">
  Buy Now — CHF 49
</a>
```

### Pattern 2: Programmatic `LemonSqueezy.Url.Open()` (for `<button>` elements)

**What:** Call `LemonSqueezy.Url.Open(checkoutUrl)` inside a JS click handler to open the overlay from any element type including `<button>`.

**When to use:** Sticky CTA button (currently `<button class="sticky-cta-btn">`) — it cannot be converted to an `<a>` without layout risk, and its onclick already contains JS logic.

```javascript
// Source: https://docs.lemonsqueezy.com/help/lemonjs/opening-overlays
const LS_CHECKOUT_URL = 'https://[STORE_SLUG].lemonsqueezy.com/checkout/buy/[VARIANT_UUID]';

document.getElementById('stickyCTAbtn').addEventListener('click', () => {
  LemonSqueezy.Url.Open(LS_CHECKOUT_URL);
});
```

### Pattern 3: Checkout.Success Event Handler (optional but recommended)

**What:** Register an event handler with `LemonSqueezy.Setup()` to react after purchase completes. Useful for showing a confirmation message without a redirect.

```javascript
// Source: https://docs.lemonsqueezy.com/help/lemonjs/handling-events
LemonSqueezy.Setup({
  eventHandler: (event) => {
    if (event.event === 'Checkout.Success') {
      // event.data is the Order object
      console.log('Purchase complete:', event.data);
      // Optionally: show a success toast / update button state
    }
  }
});
```

For this phase, a lightweight success message (e.g., briefly swap button text to "Purchase Complete ✓") is sufficient — no redirect or custom thank-you page needed.

### Checkout URL Format

```
https://[STORE_SLUG].lemonsqueezy.com/checkout/buy/[VARIANT_UUID]
```

- `STORE_SLUG` — the store's subdomain, e.g. `dimea-arcade`
- `VARIANT_UUID` — a UUID-format string identifying the specific variant (found in the LS dashboard on the product page under "Share" → "Checkout Overlay")

**Important:** Always use the `/checkout/buy/` URL, not the `/checkout/?cart=` URL. The cart URL is single-use and will break for subsequent customers.

### Current HTML Structure — What Changes

| Location | Current State | Required Change |
|----------|--------------|-----------------|
| `index.html` line 1055 — plugin section | `<a href="#" class="btn-primary">Buy Now — CHF 49</a>` | Add `lemonsqueezy-button` class, set `href` to LS checkout URL |
| `index.html` line 1055 — "Try Free" | `<a href="#" class="btn-secondary">Try Free</a>` | Set `href` to Windows demo download URL (or `#` placeholder if not yet available) |
| `index.html` line 1568–1584 — shop section | `<button id="buyPluginBtn">` + email form block + JS toggle | Replace entire block with `<a class="shop-btn shop-btn--primary lemonsqueezy-button" href="[LS_URL]">Buy Now — CHF 49 →</a>` |
| `index.html` lines 2404–2425 — JS checkout toggle | Full checkout-toggle JS block (email input, checkoutPay handler) | Remove entirely — no longer needed once LS overlay is wired |
| `index.html` line 4054 — sticky CTA button | `onclick="document.getElementById('buyPluginBtn').click();..."` | Update onclick to call `LemonSqueezy.Url.Open(LS_CHECKOUT_URL)` directly |
| `index.html` before `</body>` | No LS script | Add `<script src="https://app.lemonsqueezy.com/js/lemon.js" defer></script>` |

### Anti-Patterns to Avoid

- **Using `lemonsqueezy-button` on `<button>` elements:** Class only works on `<a>` tags. Use `LemonSqueezy.Url.Open()` for buttons instead.
- **Hardcoding the variant UUID in multiple places:** Define a single JS constant `const LS_CHECKOUT_URL = '...'` near the top of the inline script block and reference it everywhere.
- **Leaving the email-collection form in place:** The existing `#pluginCheckout` / `#buyPluginBtn` toggle JS will conflict with the LS overlay. Remove the entire custom email form and its JS event handlers.
- **Using the cart URL (`?cart=`) instead of the buy URL:** Cart URLs are single-use and will break after the first customer.
- **Adding `async` instead of `defer` to the Lemon.js script:** `defer` is the recommended attribute per LS docs — it ensures the DOM is ready before the library initializes.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Overlay modal checkout UI | Custom iframe/modal | Lemon.js built-in overlay | LS overlay handles PCI compliance, payment method display, order confirmation |
| License key delivery | Email sending, key generation | LS automated delivery | LS generates keys from product configuration and sends email automatically |
| Download file hosting | S3/file server | LS file delivery | LS hosts the installer and generates secure download links post-purchase |
| Payment processing | Stripe API calls | LS checkout | LS handles cards, PayPal, regional payment methods, VAT/taxes |

**Key insight:** The entire checkout, fulfillment, and license management stack is handled by LS. This phase is purely wiring three button elements to the correct URL and loading one script.

---

## Common Pitfalls

### Pitfall 1: `lemonsqueezy-button` Class Not Working on `<button>` Elements
**What goes wrong:** Developer adds `lemonsqueezy-button` class directly to a `<button>` — click does nothing or navigates to `#`.
**Why it happens:** The LS library initializes listeners on `<a>` elements only. `<button>` elements lack an `href` attribute, which the library reads for the checkout URL.
**How to avoid:** Convert checkout buttons to `<a>` tags, or use `LemonSqueezy.Url.Open()` in a manual JS click handler.
**Warning signs:** Click on button does nothing / navigates to `#` instead of opening overlay.

### Pitfall 2: Checkout Toggle JS Conflict
**What goes wrong:** The existing `#buyPluginBtn` click handler (toggles email form) fires alongside the LS overlay, causing both the email form to expand AND the LS overlay to open.
**Why it happens:** Both event listeners are attached to the same element.
**How to avoid:** Remove the entire custom checkout toggle block (lines 2404–2425) before or during the LS wiring. The email form and its JS are fully replaced by the LS overlay.
**Warning signs:** On click, email input expands AND LS overlay opens simultaneously.

### Pitfall 3: LS Script Loading Before DOM Ready
**What goes wrong:** `LemonSqueezy.Url.Open()` is called before Lemon.js has finished loading — `LemonSqueezy is not defined` error.
**Why it happens:** Script added without `defer`, page JS calls `Url.Open()` during load.
**How to avoid:** Always add `defer` to the Lemon.js `<script>` tag. All JS that calls `LemonSqueezy.*` must run after DOMContentLoaded (all existing inline scripts already do this — no change needed).
**Warning signs:** Console error `ReferenceError: LemonSqueezy is not defined`.

### Pitfall 4: Variant UUID Not Yet Available at Implementation Time
**What goes wrong:** Phase executes before the user has created the LS product listing — no valid checkout URL to insert.
**Why it happens:** Phase 08 has an explicit external prerequisite: "LemonSqueezy account + Arcade Chord Control product listing created."
**How to avoid:** Use a clearly labelled placeholder constant in the JS. Add a `TODO` comment so it's immediately obvious what needs filling in once LS is set up.

```javascript
// TODO: Replace with real LS checkout URL from dashboard → Share → Checkout Overlay
const LS_CHECKOUT_URL = 'https://YOUR-STORE.lemonsqueezy.com/checkout/buy/YOUR-VARIANT-UUID';
```

**Warning signs:** Clicking "Buy Now" opens the LS overlay but shows "product not found" or 404.

### Pitfall 5: Sticky CTA Delegation Breaking
**What goes wrong:** Sticky CTA's `onclick` calls `document.getElementById('buyPluginBtn').click()`, which triggers the shop-section button. After wiring, if `buyPluginBtn` is replaced with an `<a>`, `.click()` on an anchor navigates the page rather than triggering the LS overlay programmatically.
**Why it happens:** `.click()` on an `<a>` with `lemonsqueezy-button` class does fire the LS overlay, but it also scrolls the page — the combined scroll + overlay can be jarring.
**How to avoid:** Replace the sticky CTA's onclick with a direct `LemonSqueezy.Url.Open(LS_CHECKOUT_URL)` call.

---

## Code Examples

### Complete Wiring Pattern

```html
<!-- Source: https://docs.lemonsqueezy.com/guides/developer-guide/lemonjs -->

<!-- 1. Script — before </body> -->
<script src="https://app.lemonsqueezy.com/js/lemon.js" defer></script>

<!-- 2. Plugin section Buy Now — anchor with lemonsqueezy-button class -->
<a href="https://YOUR-STORE.lemonsqueezy.com/checkout/buy/YOUR-VARIANT-UUID"
   class="btn-primary lemonsqueezy-button">
  Buy Now — CHF 49
</a>

<!-- 3. Shop section Buy Now — replace <button> + email form with <a> -->
<a href="https://YOUR-STORE.lemonsqueezy.com/checkout/buy/YOUR-VARIANT-UUID"
   class="shop-btn shop-btn--primary lemonsqueezy-button">
  Buy Now — CHF 49 →
</a>

<!-- 4. Sticky CTA — keep as <button>, wire via JS -->
<button class="sticky-cta-btn" id="stickyCTAbtn">Buy Now →</button>
```

```javascript
// Source: https://docs.lemonsqueezy.com/help/lemonjs/opening-overlays

// Single source of truth for checkout URL
// TODO: Replace with real URL from LS dashboard → Share → Checkout Overlay
const LS_CHECKOUT_URL = 'https://YOUR-STORE.lemonsqueezy.com/checkout/buy/YOUR-VARIANT-UUID';

// Wire sticky CTA
const stickyCTAbtn = document.getElementById('stickyCTAbtn');
if (stickyCTAbtn) {
  stickyCTAbtn.addEventListener('click', () => {
    LemonSqueezy.Url.Open(LS_CHECKOUT_URL);
  });
}

// Optional: success handler
LemonSqueezy.Setup({
  eventHandler: (event) => {
    if (event.event === 'Checkout.Success') {
      // Lightweight confirmation — no redirect needed
      document.querySelectorAll('.lemonsqueezy-button, #stickyCTAbtn').forEach(el => {
        el.textContent = 'Purchase Complete ✓';
      });
    }
  }
});
```

### What to Remove

The following block in index.html (lines ~2404–2425) must be deleted:

```javascript
// REMOVE THIS BLOCK — replaced by LS overlay
const buyBtn = document.getElementById('buyPluginBtn');
const checkout = document.getElementById('pluginCheckout');
if (buyBtn && checkout) {
  buyBtn.addEventListener('click', () => { ... });
  document.getElementById('checkoutPay').addEventListener('click', () => { ... });
}
```

The following HTML in the shop section (lines ~1569–1582) must be deleted:

```html
<!-- REMOVE THIS BLOCK — replaced by LS overlay -->
<div class="shop-checkout" id="pluginCheckout">
  ...email form, payment methods, checkout note...
</div>
```

---

## State of the Art

| Old Approach | Current Approach | Impact |
|--------------|-----------------|--------|
| Custom email collection form + manual fulfillment | LS overlay + automated email delivery | Removes all custom checkout code; LS handles PCI, delivery, keys |
| Redirect to external LS page | Overlay modal (stays on site) | Higher conversion, no navigation away |
| `checkout.js` (older LS embed) | `lemon.js` (current official library) | `lemon.js` is the current supported name per 2025 docs |

**Note on script URL:** Some older tutorials reference `checkout.js`. The current official LS documentation uses `lemon.js` at `https://app.lemonsqueezy.com/js/lemon.js`. Use this URL.

---

## Open Questions

1. **Variant UUID availability**
   - What we know: URL format is `https://[STORE_SLUG].lemonsqueezy.com/checkout/buy/[VARIANT_UUID]`
   - What's unclear: The specific store slug and variant UUID are unknown until the user completes the LS setup (external prerequisite)
   - Recommendation: Use a named constant with a `TODO` comment as a placeholder. The plan should include an explicit task step instructing the user to paste in their real URL before the test step.

2. **"Try Free" download URL**
   - What we know: Button exists in plugin section, currently `href="#"`
   - What's unclear: Is a free demo build of Arcade Chord Control available yet?
   - Recommendation: Set href to the actual download URL if available, otherwise leave as `href="#"` with a `<!-- TODO -->` comment — this is low risk as it's a secondary CTA.

3. **`defer` interaction with existing inline scripts**
   - What we know: All existing JS is in `<script>` blocks near the end of `</body>` with no `defer` — they run after DOM is parsed
   - What's unclear: With `defer` on lemon.js, the exact execution order relative to inline scripts depends on browser — inline scripts without `defer` run before deferred external scripts in practice
   - Recommendation: Place `LemonSqueezy.Setup(...)` and the sticky CTA wiring after the `<script src="lemon.js" defer>` tag in document order, or guard with `window.addEventListener('load', ...)` if any timing issues arise. In practice `defer` + inline scripts at end of body have no conflict (LOW risk).

---

## Validation Architecture

> `nyquist_validation` key absent from config.json — treated as enabled.

### Test Framework

| Property | Value |
|----------|-------|
| Framework | Manual / browser smoke testing — no automated test framework detected |
| Config file | none |
| Quick run command | Open `index.html` in browser, click "Buy Now" |
| Full suite command | Verify all 3 buy-button touch-points open LS overlay; verify "Try Free" links correctly |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Command / Method | Notes |
|--------|----------|-----------|-----------------|-------|
| 08-01 | LS overlay opens on "Buy Now" click (plugin section) | manual smoke | Click `<a class="btn-primary lemonsqueezy-button">` | LS sandbox mode available |
| 08-02 | LS overlay opens on "Buy Now" click (shop section) | manual smoke | Click `<a class="shop-btn--primary lemonsqueezy-button">` | Same URL |
| 08-03 | LS overlay opens from sticky CTA | manual smoke | Scroll page, click sticky button | Uses `Url.Open()` |
| 08-04 | No email-collection form appears | manual smoke | Confirm `.shop-checkout` block gone | Verify HTML removal |
| 08-05 | "Try Free" button links correctly | manual smoke | Click Try Free, verify href | Placeholder or real URL |
| 08-06 | No JS console errors on page load | manual smoke | Open DevTools console, load page | Verify no `LemonSqueezy is not defined` |

### Wave 0 Gaps

- [ ] No automated test framework — all validation is manual browser smoke testing
- [ ] LS test mode: Use LS sandbox/test mode checkout URL during development to avoid real charges

*(No missing test files to create — this phase has no server-side logic)*

---

## Sources

### Primary (HIGH confidence)
- `https://docs.lemonsqueezy.com/guides/developer-guide/lemonjs` — Lemon.js script URL, `lemonsqueezy-button` class, `Url.Open()` API
- `https://docs.lemonsqueezy.com/help/lemonjs/handling-events` — `LemonSqueezy.Setup()` event handler, `Checkout.Success` event structure
- `https://docs.lemonsqueezy.com/help/lemonjs/opening-overlays` — Programmatic overlay opening via `LemonSqueezy.Url.Open()`
- `https://docs.lemonsqueezy.com/guides/developer-guide/taking-payments` — Checkout URL format (`/checkout/buy/[VARIANT_UUID]`), buy URL vs. cart URL distinction

### Secondary (MEDIUM confidence)
- `https://jakobgreenfeld.com/carrd-lemonsqueezy` — Confirmed class must be on `<a>` elements, not `<button>` elements; confirmed `checkout.js` alternative script name (older)
- `https://symfonycasts.com/screencast/lemon-squeezy/checkout-overlay` — Confirmed `lemon.js` as current script, step-by-step overlay setup

### Tertiary (LOW confidence — supporting only)
- WebSearch results corroborating `lemonsqueezy-button` class pattern and CDN URL

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — lemon.js confirmed via official LS docs
- Architecture: HIGH — overlay pattern confirmed via official docs + two secondary sources
- Pitfalls: MEDIUM — button vs. anchor limitation confirmed by multiple sources; timing pitfall is standard JS loading knowledge
- HTML change map: HIGH — read directly from index.html source

**Research date:** 2026-03-14
**Valid until:** 2026-06-14 (LS API stable; re-verify if LS announces breaking changes to lemon.js)
