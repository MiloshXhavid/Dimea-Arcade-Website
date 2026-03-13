# Phase 03: Legal Pages - Research

**Researched:** 2026-03-13
**Domain:** Static HTML legal pages — privacy policy, terms of service, refund policy
**Confidence:** HIGH

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Visual Design**
- Theme: Clean minimal — white/light background, dark text. NOT dark-themed.
- Fonts: Syne 700 for headings, Space Grotesk for body (same font imports as main site), teal (#2ae8c4) for links on white background.
- Chrome: Minimal — a simple "← Back to dimea.audio" link at the top. No full nav, no footer columns.
- Navigation: Links open in the same tab (standard internal page behavior).
- Print mode: Not needed. No print CSS.

**Legal Entity & Jurisdiction**
- Legal name: "Dimea Arcade, operated by Dimitri Dähler" — both brand name and natural person.
- Registration status: Individual / sole trader, not a registered GmbH or Einzelfirma. No CHE number.
- Impressum: Folded into privacy.html under an "About this Site / Data Controller" section. No separate impressum.html.
- Contact details: Email placeholder only — `[contact@dimea.audio]` (to be updated at launch when domain is confirmed). No physical address.
- Legal framework: Swiss DSG (nDSG) + GDPR — both referenced, covers Swiss-based operator selling to EU customers.
- Language: English only.

**Refund Policy**
- Plugin (Arcade Chord Control): 14-day no-questions refund. Customer emails `[contact@dimea.audio]` with order number. Handled manually via LemonSqueezy dashboard.
- License after refund: License key revoked immediately upon refund.
- Hardware deposit (DIMEOLA): Deposit non-refundable if customer cancels. BUT: if Dimea Arcade cancels the hardware project, all deposits are refunded in full. State this explicitly.
- Deposit percentage: ~20–30% of CHF 499 (exact amount TBD — leave as placeholder).

**Footer & Navigation**
- Footer links: Add "Refund" as a third link — `.footer-legal` becomes: Privacy · Terms · Refund
- Link targets: All three are same-tab (`href="privacy.html"`, not `href="#"`)
- Shop cards: No refund links near buy buttons — footer placement only.
- Plausible note: Mentioned in privacy.html only (not footer). Mark as "will be added when Plausible is installed" — placeholder section in privacy policy.

**Third-Party Data Processors (active at launch)**
- LemonSqueezy — Plugin payments. Collects: name, email, payment details. Acts as Merchant of Record (handles VAT). Privacy policy: lemonsqueezy.com/privacy
- Mailchimp — Email newsletter/waitlist (footer signup). Collects: email address only.
- Stripe — Hardware deposit payments via Payment Links. Collects: name, email, payment details.
- Coinbase Commerce — Crypto payments (BTC/ETH/USDC/SOL). Collects: wallet address, transaction data.
- Plausible Analytics — NOT active at launch. Add to privacy policy when installed.

**Policy Updates**
- Notification: Email Mailchimp subscribers when policies change significantly.
- Version tracking: Show "Last updated: [Month Year]" at top of each policy page.

### Claude's Discretion
- Exact wording of legal copy (standard boilerplate is fine — this is an indie operation, not a legal firm)
- Section order within each document
- Exact HTML structure and CSS implementation
- Whether to use a shared CSS block across all three pages or inline per-file

### Deferred Ideas (OUT OF SCOPE)
- German language versions (/datenschutz.html, /agb.html, /rueckgabe.html) — future phase if Swiss/German market becomes priority
- Physical address in Impressum — add when/if operating address is established
- Plausible Analytics integration — add to privacy policy in the same phase Plausible is installed
</user_constraints>

---

## Summary

Phase 03 creates three standalone HTML legal pages (privacy.html, terms.html, refund.html) required before LemonSqueezy plugin sales and Stripe hardware deposits can go live. The deliverables are purely content pages — no JavaScript, no build tooling, no external dependencies beyond the Google Fonts already used on the main site.

The project uses flat HTML files (index.html, manual.html) with self-contained per-file CSS and no bundler. Legal pages follow the same pattern but use a light/white theme rather than the dark navy brand theme of the main site. The manual.html file is the closest structural reference: self-contained, same font imports, standalone page.

LemonSqueezy acts as Merchant of Record for plugin sales, meaning it handles VAT collection and payment processing — which simplifies what the seller's Terms of Service need to say about taxes. Stripe is used directly (not as MoR) for hardware deposits, so the privacy policy must explicitly mention both processors. The primary legal frameworks are Swiss nDSG (in force since September 2023) and GDPR (applicable because EU customers will be served).

**Primary recommendation:** Write three self-contained HTML files with inline `<style>` blocks, white-background light theme, shared font import string, and legally sufficient but plainly written boilerplate copy. Update the `.footer-legal` div in index.html to add the Refund link and wire all three hrefs.

---

## Standard Stack

### Core

| Technology | Version | Purpose | Why Standard |
|------------|---------|---------|--------------|
| Plain HTML5 | — | Page structure | Matches existing project pattern (index.html, manual.html) |
| Inline CSS | — | Per-file styles | No build step; project convention |
| Google Fonts | — | Syne + Space Grotesk | Already imported on main site and manual.html |

### No Libraries Needed

This phase requires zero npm packages. No JavaScript is needed. No build step. The pattern is exactly manual.html: a single self-contained .html file.

### Supporting

| Tool | Purpose | When to Use |
|------|---------|-------------|
| W3C Validator | Confirm valid HTML | Optional sanity check after writing |
| Browser devtools | Verify light-theme rendering | After writing, before commit |

---

## Architecture Patterns

### Recommended Project Structure

```
dimea-website/
├── index.html          # existing — update .footer-legal links
├── manual.html         # reference pattern for standalone pages
├── privacy.html        # NEW — Privacy Policy + Impressum/Data Controller
├── terms.html          # NEW — Terms of Service
└── refund.html         # NEW — Refund Policy
```

All three legal pages live at root level, same directory as index.html and manual.html.

### Pattern 1: Self-Contained HTML File with Light Theme

**What:** Each legal page is a single .html file with a `<style>` block in `<head>`. No external CSS file, no framework.

**When to use:** Always — this is the established project convention.

**Structure per file:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Page Title] — Dimea Arcade</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@400;500&family=Space+Grotesk:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <style>
    /* light theme variables */
    :root {
      --teal: #2ae8c4;
      --text: #111827;
      --text-dim: #4b5563;
      --bg: #ffffff;
    }
    body { background: var(--bg); color: var(--text); font-family: 'Space Grotesk', sans-serif; }
    /* ... scoped styles ... */
  </style>
</head>
<body>
  <div class="back-link"><a href="index.html">← Back to dimea.audio</a></div>
  <main class="legal-content">
    <h1>[Policy Title]</h1>
    <p class="last-updated">Last updated: March 2026</p>
    <!-- policy sections -->
  </main>
</body>
</html>
```

**Note:** The font import string used in manual.html (`@import url(...)` inside `<style>`) also works, but the `<link>` approach in `<head>` is slightly faster for initial paint. Either is acceptable.

### Pattern 2: Footer Link Update in index.html

**What:** The existing `.footer-legal` div has two `<a href="#">` placeholders. Update all three to real paths and add the Refund link.

**Current state (lines ~1975–1978 of index.html):**
```html
<div class="footer-legal">
  <a href="#">Privacy</a>
  <a href="#">Terms</a>
  <a href="/cdn-cgi/l/email-protection#...">Contact</a>
</div>
```

**Target state:**
```html
<div class="footer-legal">
  <a href="privacy.html">Privacy</a>
  <a href="terms.html">Terms</a>
  <a href="refund.html">Refund</a>
  <a href="mailto:contact@dimea.audio">Contact</a>
</div>
```

Note: The Contact link is currently obfuscated by Cloudflare email protection — keep that link as-is or update it alongside the legal links. The main tasks are the three legal hrefs and adding Refund.

### Pattern 3: CSS Approach for Light Theme

**What:** Legal pages use white background, dark text — the inverse of the main site's dark navy. Reuse teal (#2ae8c4) for links only.

**Recommended minimal CSS variables for legal pages:**
```css
:root {
  --teal: #2ae8c4;
  --text-primary: #111827;
  --text-secondary: #4b5563;
  --bg: #ffffff;
  --border: #e5e7eb;
}
body {
  background: var(--bg);
  color: var(--text-primary);
  font-family: 'Space Grotesk', sans-serif;
  font-size: 16px;
  line-height: 1.7;
  max-width: 720px;
  margin: 0 auto;
  padding: 48px 24px 80px;
}
h1 { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 2rem; margin-bottom: 8px; }
h2 { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 1.25rem; margin: 2rem 0 0.75rem; }
a { color: var(--teal); }
```

**Key decision (Claude's discretion):** Use per-file inline CSS (no shared stylesheet). This keeps each page fully self-contained, consistent with manual.html, and avoids needing to create a separate legal.css file with its own caching considerations.

### Anti-Patterns to Avoid

- **Dark theme on legal pages:** Legal pages deliberately use white/light background for readability. Do NOT copy the dark navy CSS variables from index.html.
- **Full nav bar:** No full navigation header — only a simple "← Back to dimea.audio" text link. Do not replicate the complex nav component.
- **Print CSS:** Explicitly not required per user decisions.
- **Separate impressum.html:** The Impressum/Data Controller section lives inside privacy.html. No separate file.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Legal copy authoring | Custom policy text from scratch | Standard indie-developer boilerplate, adapted | Edge cases in legal language are subtle; boilerplate covers known requirements |
| Cookie consent banner | Any JS consent popup | Nothing (Plausible is not yet installed; no cookies present at launch) | No cookies = no consent requirement under GDPR/nDSG |
| CSS framework | Bootstrap, Tailwind import | Inline `<style>` block | Project convention; no build step; pages are simple enough |

**Key insight:** LemonSqueezy as Merchant of Record substantially reduces what the Terms of Service must cover — LemonSqueezy handles VAT, payment processing, and accepts chargebacks on behalf of sellers. The seller's terms mainly need to cover: product description, license grant, refund policy, and acceptable use. Tax and payment processing is LemonSqueezy's responsibility to state in their buyer terms.

---

## Common Pitfalls

### Pitfall 1: Forgetting Coinbase Commerce in Privacy Policy

**What goes wrong:** Privacy policy lists LemonSqueezy, Mailchimp, Stripe — but omits Coinbase Commerce (crypto payments).
**Why it happens:** Crypto option is less prominent in the mental model.
**How to avoid:** Use the exact list from CONTEXT.md as a checklist: LemonSqueezy + Mailchimp + Stripe + Coinbase Commerce + (Plausible — placeholder).
**Warning signs:** Privacy policy section count for "Third-Party Services" is fewer than 4 processors.

### Pitfall 2: Using `href="#"` Placeholders in Footer

**What goes wrong:** index.html footer still has `href="#"` on Privacy and Terms after the task completes.
**Why it happens:** Forgetting to update the existing file when creating new ones.
**How to avoid:** Plan 03-02 explicitly covers footer link wiring. Verify both old links (Privacy, Terms) are updated AND new Refund link is added.
**Warning signs:** Clicking Privacy or Terms in footer navigates to top of page (# behavior).

### Pitfall 3: License Revocation Language Missing from Refund Policy

**What goes wrong:** Refund policy states the 14-day window but omits that the license key is revoked upon refund.
**Why it happens:** Refund mechanics are straightforward; license management is easy to forget.
**How to avoid:** Refund policy must include explicit sentence: "Upon issuing a refund, your license key will be deactivated and access to software activations will be revoked."
**Warning signs:** No mention of license/activation in refund.html.

### Pitfall 4: Hardware Cancellation Clause Missing

**What goes wrong:** Refund policy says deposits are non-refundable but omits the Dimea Arcade cancellation scenario.
**Why it happens:** The non-refundable rule is the main point; the edge case is easy to skip.
**How to avoid:** CONTEXT.md is explicit: "If Dimea Arcade cancels the DIMEOLA hardware project, all deposits will be refunded in full." This sentence must appear verbatim or close to it.
**Warning signs:** No "if we cancel" exception in the hardware deposit section.

### Pitfall 5: Dark Theme CSS Leaking Into Legal Pages

**What goes wrong:** Developer copies CSS block from index.html as a starting point, carrying over `--navy-deep`, dark body background, light `--text` color.
**Why it happens:** index.html is the most familiar file to copy from.
**How to avoid:** Use manual.html as the structural reference, but define new light-theme variables. Do not copy `:root` from index.html.
**Warning signs:** Legal page renders with dark navy background in browser.

---

## Legal Content Requirements

### privacy.html — Required Sections

| Section | Requirement | Notes |
|---------|-------------|-------|
| Data Controller / About this Site | Name + contact | "Dimea Arcade, operated by Dimitri Dähler · [contact@dimea.audio]" — this doubles as Impressum |
| What Data We Collect | Enumerate by category | Via processors: payment data (LS/Stripe/Coinbase), email (Mailchimp) |
| Third-Party Processors | List all 4 active | LemonSqueezy, Mailchimp, Stripe, Coinbase Commerce |
| Plausible Analytics Placeholder | Future section | "We plan to use Plausible Analytics (cookie-free) — this section updated when activated" |
| Cookies | State: no first-party cookies | Payment processors may use their own cookies |
| Data Rights (GDPR/nDSG) | Access, deletion, portability | Contact via email to exercise |
| Policy Updates | Notification via Mailchimp | "Last updated: [Month Year]" at top |
| Legal Framework | Reference both | Swiss nDSG (FADP) + GDPR |

### terms.html — Required Sections

| Section | Requirement | Notes |
|---------|-------------|-------|
| Products Covered | Plugin + hardware | Arcade Chord Control (software), DIMEOLA (hardware, pre-order) |
| License Grant | Plugin: personal, non-transferable | Single-user license; no redistribution |
| Payment Processor | LemonSqueezy as MoR | They handle tax, billing — reference their buyer terms |
| Refund Reference | Link/point to refund.html | Or summarize inline |
| Acceptable Use | No reverse engineering, redistribution | Standard software license clause |
| Disclaimer of Warranties | "As is" | Standard for indie software |
| Limitation of Liability | Cap at purchase price | Standard |
| Governing Law | Switzerland | |
| Contact | Email placeholder | `[contact@dimea.audio]` |

### refund.html — Required Sections

| Section | Requirement | Notes |
|---------|-------------|-------|
| Plugin Refunds | 14-day no-questions | Customer emails with order number |
| License Revocation | On refund, key deactivated | Explicit sentence required |
| Hardware Deposit | Non-refundable if customer cancels | Clear bold notice |
| Dimea Arcade Cancellation | Full deposit refund | "If we cancel DIMEOLA, all deposits refunded in full" |
| How to Request | Email [contact@dimea.audio] | Process: email with order number |
| Processing Time | State a timeframe | e.g., "within 5–10 business days via original payment method" |

---

## Code Examples

### Back Link Pattern
```html
<!-- Source: project convention (no framework) -->
<div class="back-link">
  <a href="index.html">← Back to dimea.audio</a>
</div>
```

### Last Updated Header Pattern
```html
<h1>Privacy Policy</h1>
<p class="meta">Last updated: March 2026</p>
```

### Third-Party Processor Entry Pattern (Privacy Policy)
```html
<h2>Third-Party Services</h2>
<p>We use the following third-party services that may process your personal data:</p>
<ul>
  <li><strong>LemonSqueezy</strong> (Merchant of Record) — processes plugin purchases, collects name, email, and payment details. <a href="https://www.lemonsqueezy.com/privacy" target="_blank">Privacy Policy</a></li>
  <li><strong>Stripe</strong> — processes hardware deposit payments, collects name, email, and payment details. <a href="https://stripe.com/privacy" target="_blank">Privacy Policy</a></li>
  <li><strong>Mailchimp</strong> — manages email newsletter signups, collects email address only. <a href="https://mailchimp.com/legal/privacy/" target="_blank">Privacy Policy</a></li>
  <li><strong>Coinbase Commerce</strong> — processes cryptocurrency payments, collects wallet address and transaction data. <a href="https://www.coinbase.com/legal/privacy" target="_blank">Privacy Policy</a></li>
</ul>
```

### Hardware Deposit Refund Clause (Critical)
```html
<!-- refund.html — hardware section -->
<h2>Hardware Deposit (DIMEOLA)</h2>
<p><strong>Deposits are non-refundable if you cancel your reservation.</strong> The deposit
secures your place in the production run and covers initial manufacturing costs.</p>
<p>However, if Dimea Arcade cancels the DIMEOLA hardware project for any reason, all
deposits will be refunded in full via the original payment method.</p>
```

### Footer Link Update (index.html)
```html
<!-- Target state for .footer-legal -->
<div class="footer-legal">
  <a href="privacy.html">Privacy</a>
  <a href="terms.html">Terms</a>
  <a href="refund.html">Refund</a>
  <a href="mailto:contact@dimea.audio">Contact</a>
</div>
```

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Cookie consent banners required | Not required if no first-party cookies or tracking | GDPR clarification + Plausible adoption | Pages with Plausible (cookie-free analytics) skip consent banners entirely |
| Swiss DSG (old) | nDSG / FADP (new) | September 2023 | Closer alignment with GDPR; privacy policy must name all data categories |
| LemonSqueezy as payment gateway | LemonSqueezy as Merchant of Record | Ongoing | Seller's terms need less detail on tax/VAT — LemonSqueezy assumes that liability |

**Not needed at this stage:**
- Cookie consent popup (Plausible not installed; no first-party cookies)
- German translations (deferred)
- Separate impressum.html (folded into privacy.html)

---

## Open Questions

1. **Contact email domain**
   - What we know: Placeholder `[contact@dimea.audio]` throughout
   - What's unclear: Whether domain is confirmed before launch
   - Recommendation: Use the placeholder as written; CONTEXT.md acknowledges this will be updated at launch

2. **Hardware deposit exact amount**
   - What we know: "~20–30% of CHF 499" — not finalized
   - What's unclear: Exact deposit figure
   - Recommendation: Write refund.html with "approximately CHF 100–150" or use a placeholder bracket like `[DEPOSIT_AMOUNT]`; update when confirmed

3. **Coinbase Commerce privacy policy URL**
   - What we know: Coinbase Commerce is listed as a processor
   - What's unclear: Whether the correct link is coinbase.com/legal/privacy or a Commerce-specific URL
   - Recommendation: Use `https://www.coinbase.com/legal/privacy` as the general Coinbase privacy link; LOW confidence — verify before launch

---

## Validation Architecture

No automated test framework exists in this project (plain HTML/CSS, no package.json, no test runner). This phase is not amenable to automated testing — there is no build step to hook into.

### Manual Verification Checklist (per task commit)

| Check | Task | Method |
|-------|------|--------|
| Page renders with white background | 03-01, 03-02, 03-03 | Open in browser, confirm no dark background |
| Back link navigates to index.html | 03-01, 03-02, 03-03 | Click link, confirm navigation |
| All four third-party processors listed | 03-01 (privacy.html) | Read through processors section |
| Hardware cancellation clause present | 03-02 (refund.html) | Search for "Dimea Arcade cancels" |
| License revocation clause present | 03-02 (refund.html) | Search for "license" or "revoked" |
| Footer has 3 legal links + Contact | 03-02 (index.html) | Check footer in browser |
| No `href="#"` placeholders remain | 03-02 (index.html) | Inspect `.footer-legal` links |
| Fonts load (Syne + Space Grotesk) | All | Open browser devtools Network tab |

### Wave 0 Gaps

None — this phase creates new HTML files. No test infrastructure is required or appropriate for static content pages. Verification is entirely manual browser inspection.

---

## Sources

### Primary (HIGH confidence)
- Project CONTEXT.md — all legal entity details, processor list, refund rules
- index.html (lines 1975–1978) — existing footer structure, `.footer-legal` CSS
- manual.html (lines 1–60) — reference pattern for standalone HTML file with font imports

### Secondary (MEDIUM confidence)
- [LemonSqueezy Merchant of Record docs](https://docs.lemonsqueezy.com/help/payments/merchant-of-record) — MoR handles VAT/tax; seller terms simplified
- [LemonSqueezy Refunds and Chargebacks docs](https://docs.lemonsqueezy.com/help/payments/refunds-chargebacks) — refund window and process
- [Stripe privacy requirements for sellers](https://usercentrics.com/guides/privacy-policies-of-major-platforms/stripe-privacy-policy/) — recommended disclosure language
- [Swiss nDSG/FADP overview](https://www.kmu.admin.ch/kmu/en/home/facts-and-trends/digitization/data-protection/new-federal-act-on-data-protection-nfadp.html) — in force September 2023, GDPR-aligned

### Tertiary (LOW confidence)
- Coinbase Commerce privacy URL — assumed to be coinbase.com/legal/privacy; not verified against Commerce-specific page

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — plain HTML/CSS is verified project convention; no ambiguity
- Architecture: HIGH — directly observed from existing files (manual.html pattern)
- Legal content requirements: MEDIUM — based on standard indie-software practice and locked decisions from CONTEXT.md; not reviewed by a lawyer
- Pitfalls: HIGH — derived from explicit CONTEXT.md requirements and direct code inspection

**Research date:** 2026-03-13
**Valid until:** 2026-06-13 (stable domain — legal frameworks don't change frequently; LemonSqueezy MoR model stable)
