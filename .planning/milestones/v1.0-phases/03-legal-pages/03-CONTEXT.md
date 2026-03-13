# Phase 03: Legal Pages — Context

**Gathered:** 2026-03-13
**Status:** Ready for planning

<domain>
## Phase Boundary

Three standalone HTML pages (privacy.html, terms.html, refund.html) required before LemonSqueezy + Stripe payments go live. Footer links in index.html wired to all three. No new capabilities — purely legal content pages.

</domain>

<decisions>
## Implementation Decisions

### Visual Design
- **Theme:** Clean minimal — white/light background, dark text. NOT dark-themed. Legal pages use standard readable styling, not the dark navy brand.
- **Fonts:** Syne 700 for headings, Space Grotesk for body (same font imports as main site), teal (#2ae8c4) for links on white background.
- **Chrome:** Minimal — a simple "← Back to dimea.audio" link at the top. No full nav, no footer columns.
- **Navigation:** Links open in the same tab (standard internal page behavior).
- **Print mode:** Not needed. No print CSS.

### Legal Entity & Jurisdiction
- **Legal name:** "Dimea Arcade, operated by Dimitri Dähler" — both brand name and natural person.
- **Registration status:** Individual / sole trader, not a registered GmbH or Einzelfirma. No CHE number.
- **Impressum:** Folded into privacy.html under an "About this Site / Data Controller" section. No separate impressum.html.
- **Contact details:** Email placeholder only — `[contact@dimea.audio]` (to be updated at launch when domain is confirmed). No physical address.
- **Legal framework:** Swiss DSG (nDSG) + GDPR — both referenced, covers Swiss-based operator selling to EU customers.
- **Language:** English only.

### Refund Policy
- **Plugin (Arcade Chord Control):** 14-day no-questions refund. Customer emails `[contact@dimea.audio]` with order number. Handled manually via LemonSqueezy dashboard.
- **License after refund:** License key revoked immediately upon refund.
- **Hardware deposit (DIMEOLA):** Deposit non-refundable if customer cancels. BUT: if Dimea Arcade cancels the hardware project, all deposits are refunded in full. State this explicitly.
- **Deposit percentage:** ~20–30% of CHF 499 (exact amount TBD — leave as placeholder or pull from hardware section copy).

### Footer & Navigation
- **Footer links:** Add "Refund" as a third link — `.footer-legal` becomes: Privacy · Terms · Refund
- **Link targets:** All three are same-tab (`href="privacy.html"`, not `href="#"`)
- **Shop cards:** No refund links near buy buttons — footer placement only.
- **Plausible note:** Mentioned in privacy.html only (not footer). Mark as "will be added when Plausible is installed" — placeholder section in privacy policy.

### Third-Party Data Processors (active at launch)
- **LemonSqueezy** — Plugin payments. Collects: name, email, payment details. Acts as Merchant of Record (handles VAT). Privacy policy: lemon squeezy.com/privacy
- **Mailchimp** — Email newsletter/waitlist (footer signup). Collects: email address only.
- **Stripe** — Hardware deposit payments via Payment Links. Collects: name, email, payment details.
- **Coinbase Commerce** — Crypto payments (BTC/ETH/USDC/SOL). Collects: wallet address, transaction data.
- **Plausible Analytics** — NOT active at launch. Add to privacy policy when installed.

### Policy Updates
- **Notification:** Email Mailchimp subscribers when policies change significantly.
- **Version tracking:** Show "Last updated: [Month Year]" at top of each policy page.

### Claude's Discretion
- Exact wording of legal copy (standard boilerplate is fine — this is an indie operation, not a legal firm)
- Section order within each document
- Exact HTML structure and CSS implementation
- Whether to use a shared CSS block across all three pages or inline per-file

</decisions>

<code_context>
## Existing Code Insights

### Reusable Assets
- **Font imports:** `@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@400;500&family=Space+Grotesk:wght@300;400;500;600;700&display=swap')` — same import string used in manual.html and index.html
- **CSS variables (main site):** `--teal: #2ae8c4`, `--magenta: #e83a6e`, `--navy-deep: #080c18` — legal pages use white bg but can reuse teal for links
- **manual.html:** Existing standalone HTML page pattern — self-contained CSS, no build step, same font imports. Legal pages follow same structure but lighter theme.

### Established Patterns
- **Standalone HTML files:** The project uses flat HTML files (index.html, manual.html). Legal pages are `privacy.html`, `terms.html`, `refund.html` in the root directory.
- **No build pipeline:** Plain HTML/CSS, no bundler. Self-contained per-file styles.

### Integration Points
- **Footer `.footer-legal` div** — Currently has `<a href="#">Privacy</a>` and `<a href="#">Terms</a>`. Update to: `privacy.html`, `terms.html`, add `refund.html`.
- **Location:** Root directory (same level as index.html, manual.html)

</code_context>

<specifics>
## Specific Ideas

- The refund policy must explicitly state: "If Dimea Arcade cancels the DIMEOLA hardware project, all deposits will be refunded in full."
- Impressum/data controller section in privacy.html: "Dimea Arcade, operated by Dimitri Dähler · [contact@dimea.audio]"
- Plausible section in privacy.html: placeholder with note "We plan to use Plausible Analytics (cookie-free, no personal data collected) — this section will be updated when activated."
- Policy update notification: send to Mailchimp list when significant changes occur

</specifics>

<deferred>
## Deferred Ideas

- German language versions (/datenschutz.html, /agb.html, /rueckgabe.html) — future phase if Swiss/German market becomes priority
- Physical address in Impressum — add when/if operating address is established
- Plausible Analytics integration — add to privacy policy in the same phase Plausible is installed

</deferred>

---

*Phase: 03-legal-pages*
*Context gathered: 2026-03-13*
