---
phase: 03-legal-pages
plan: 01
subsystem: ui
tags: [html, css, legal, privacy, terms, gdpr, ndsg, lemonsqueezy, stripe]

# Dependency graph
requires: []
provides:
  - "privacy.html — GDPR/nDSG compliant Privacy Policy with Impressum and four active data processor disclosures"
  - "terms.html — Terms of Service with plugin license grant, LemonSqueezy MoR disclosure, and Swiss governing law"
affects: [03-legal-pages, footer-links, lemonsqueezy-setup, stripe-setup]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "Standalone HTML legal pages — self-contained per-file CSS, no build step, light theme (white bg, dark text)"
    - "Light theme CSS variables: --bg #ffffff, --text-primary #111827, --teal #2ae8c4 for links on white"

key-files:
  created:
    - privacy.html
    - terms.html
  modified: []

key-decisions:
  - "Impressum folded into privacy.html under 'About this Site / Data Controller' section — no separate impressum.html"
  - "Light theme (white bg, dark text) chosen for legal pages — distinct from main site dark navy brand"
  - "Identical CSS block duplicated across both files — no shared stylesheet, consistent with flat HTML project pattern"
  - "Plausible Analytics added as placeholder section in privacy.html — marked pending activation, not yet live"
  - "No first-party cookies statement included at launch — processors handle their own cookies"

patterns-established:
  - "Legal page pattern: self-contained HTML, identical CSS vars, .back-link + .meta at top, Syne headings + Space Grotesk body"

requirements-completed: []

# Metrics
duration: 12min
completed: 2026-03-13
---

# Phase 03 Plan 01: Legal Pages (Privacy + Terms) Summary

**Two standalone GDPR/nDSG-compliant HTML legal pages — Privacy Policy with Impressum and Coinbase/Mailchimp/Stripe/LemonSqueezy processor disclosures, plus Terms of Service with non-transferable plugin license grant and Swiss governing law.**

## Performance

- **Duration:** ~12 min
- **Started:** 2026-03-13T19:05:24Z
- **Completed:** 2026-03-13T19:17:00Z
- **Tasks:** 2
- **Files modified:** 2

## Accomplishments

- privacy.html created with all four active data processor disclosures (LemonSqueezy, Stripe, Mailchimp, Coinbase Commerce), Impressum section, Plausible placeholder, GDPR/nDSG rights, and no first-party cookies statement
- terms.html created with non-transferable single-user plugin license grant, LemonSqueezy as Merchant of Record, refund.html cross-reference, and Swiss governing law
- Both pages use light theme (white background, dark text) — distinct from main site dark navy — with Syne headings and Space Grotesk body text

## Task Commits

Each task was committed atomically:

1. **Task 1: Create privacy.html — Privacy Policy + Impressum** - `225cbf3` (feat)
2. **Task 2: Create terms.html — Terms of Service** - `07e1e3e` (feat)

**Plan metadata:** (docs commit — pending)

## Files Created/Modified

- `privacy.html` — Privacy Policy + Impressum; 137 lines; GDPR/nDSG compliant; four processor disclosures; Plausible placeholder; data controller section
- `terms.html` — Terms of Service; 150 lines; plugin license grant; LemonSqueezy MoR; refund.html cross-reference; Swiss governing law

## Decisions Made

- Impressum folded into privacy.html under "About this Site / Data Controller" — Swiss indie operators without a registered company typically combine Impressum with privacy policy rather than creating a separate page.
- Identical CSS block duplicated across both files — project uses flat HTML with no build pipeline, so a shared stylesheet would require an extra HTTP request and a new file. Per-file CSS is consistent with the existing manual.html pattern.
- Plausible Analytics kept as a placeholder section rather than being omitted — makes future activation a one-line edit rather than requiring re-reading the entire privacy policy structure.
- teal (#2ae8c4) used for links on white background — same brand teal, readable on white (contrast ratio sufficient), consistent with main site brand color.

## Deviations from Plan

None — plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None — no external service configuration required. The `refund.html` link in terms.html will resolve when plan 03-02 (or a subsequent plan) creates that file.

## Next Phase Readiness

- privacy.html and terms.html are ready to be linked from the footer (plan 03-02 will update index.html footer links)
- refund.html referenced by terms.html — must be created before terms page is published to avoid broken link
- Both pages fully compliant for LemonSqueezy and Stripe seller requirements

---
*Phase: 03-legal-pages*
*Completed: 2026-03-13*
