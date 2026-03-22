# Phase 08: LemonSqueezy Plugin Checkout — Context

**Gathered:** 2026-03-14
**Status:** Ready for planning
**Source:** Conversation context (user decisions)

<domain>
## Phase Boundary

Wire LemonSqueezy overlay checkout into index.html so visitors can buy Arcade Chord Control directly on the site without leaving the page. This phase is website-only — LemonSqueezy account setup and product creation are external prerequisites done by the user before execution.

</domain>

<decisions>
## Implementation Decisions

### Checkout Method
- Overlay (not redirect) — keeps user on site, higher conversion rate
- LemonSqueezy supports overlay via their JS embed snippet + `data-lemon-squeezy-checkout` attribute on the button

### Pricing
- Launch price: CHF 49
- Regular price: CHF 79 (configured in LemonSqueezy, not hardcoded on site)
- Site shows CHF 49 launch price during launch period

### Button Wiring
- "Buy Now" button in the plugin section → LS overlay
- No new buttons added — existing button gets LS overlay attribute
- "Try Free" button → demo download link (also wired in this phase if link is available, otherwise placeholder)

### Post-purchase
- LS handles license key + download link delivery by email automatically
- No custom thank-you page needed for this phase

### External Prerequisites (NOT part of this phase)
- LemonSqueezy account created
- Arcade Chord Control product listed with correct pricing
- Windows installer (DimaChordJoystick-Setup.exe) uploaded to LS
- Product URL/store slug known before execution

### Claude's Discretion
- Exact JS snippet placement in index.html (before </body> preferred)
- Whether to show a "Powered by LemonSqueezy" note near button
- Button loading state while LS overlay initializes

</decisions>

<specifics>
## Specific Details

- Plugin name: Arcade Chord Control
- Seller: Dimea Arcade
- Platform: LemonSqueezy (not Gumroad, not Stripe directly)
- Site: flat HTML, no framework — vanilla JS only
- Buy button currently exists in index.html plugin section (placeholder href)
- Site hosted on Vercel (static)

</specifics>

<deferred>
## Deferred Ideas

- Mac installer upload (waiting for Mac signing ~2026-03-21)
- Crypto checkout option alongside LS (Phase 11)
- License key validation/activation on site (not needed at launch)
- Affiliate/referral tracking (future)

</deferred>

---

*Phase: 08-lemonsqueezy-plugin-checkout*
*Context gathered: 2026-03-14 via conversation*
