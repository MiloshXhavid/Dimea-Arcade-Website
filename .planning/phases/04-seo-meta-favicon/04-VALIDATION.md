---
phase: 04
slug: seo-meta-favicon
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-03-14
---

# Phase 04 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | None — flat static HTML, no test runner installed |
| **Config file** | none |
| **Quick run command** | Manual: open index.html in Chrome, view-source for og:image URL |
| **Full suite command** | Manual checklist (see Per-Task Verification Map below) |
| **Estimated runtime** | ~5 minutes manual |

---

## Sampling Rate

- **After every task commit:** Open in Chrome, check browser tab favicon, view-source and grep `og:image` content
- **After every plan wave:** Full manual checklist (all rows below), Discord preview test, schema.org validator
- **Before `/gsd:verify-work`:** All manual checks green — domain must be live or Vercel preview URL used for social card testing

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 04-01a | 01 | 1 | `<title>` present and ≤60 chars | manual | Open index.html, check browser tab | ❌ no test infra | ⬜ pending |
| 04-01b | 01 | 1 | `og:image` uses absolute HTTPS URL | manual | view-source, grep `og:image` content | ❌ no test infra | ⬜ pending |
| 04-01c | 01 | 1 | OG tags parse correctly on Discord/Twitter | manual | Paste URL into Discord DM; cards-dev.twitter.com validator | ❌ requires live domain | ⬜ pending |
| 04-01d | 01 | 1 | JSON-LD validates with no errors | manual | Paste into validator.schema.org | ❌ no test infra | ⬜ pending |
| 04-01e | 01 | 1 | sitemap.xml accessible and valid XML | manual | Open sitemap.xml in browser | ❌ Wave 0 | ⬜ pending |
| 04-01f | 01 | 1 | robots.txt accessible and correct | manual | Open robots.txt in browser | ❌ Wave 0 | ⬜ pending |
| 04-02a | 02 | 2 | Favicon displays in browser tab | manual | Open index.html in Chrome, Firefox, Safari | ❌ Wave 0 | ⬜ pending |
| 04-02b | 02 | 2 | apple-touch-icon renders on iOS home screen | manual | iOS Safari — Add to Home Screen | ❌ Wave 0 | ⬜ pending |
| 04-02c | 02 | 2 | OG image is 1200×630 | manual | Inspect image dimensions in browser dev tools | ❌ Wave 0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] `sitemap.xml` — covers REQ 04-01e
- [ ] `robots.txt` — covers REQ 04-01f
- [ ] `favicon.ico` (32×32 ICO) — covers REQ 04-02a
- [ ] `icon.svg` (scalable, dark mode aware) — covers REQ 04-02a
- [ ] `apple-touch-icon.png` (180×180 PNG) — covers REQ 04-02b
- [ ] `assets/og-image.png` (1200×630 PNG) — covers REQ 04-02c

*Note: These are deliverable files, not test stubs. No test framework exists — validation is entirely manual for this flat HTML project.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| OG preview on Discord | 04-01c | Requires live domain; no headless tool reliable enough | Paste URL into Discord DM, screenshot preview |
| Twitter card preview | 04-01c | Requires live URL | Use cards-dev.twitter.com with Vercel preview URL |
| JSON-LD rich results | 04-01d | Static analysis insufficient; Google renderer needed | Paste JSON-LD into validator.schema.org |
| Favicon in all browsers | 04-02a | Cross-browser visual check | Open in Chrome, Firefox, Safari — verify tab icon |
| iOS home screen icon | 04-02b | Device-specific, no automation | iOS Safari: Add to Home Screen, check icon |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 300s (manual checks)
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
