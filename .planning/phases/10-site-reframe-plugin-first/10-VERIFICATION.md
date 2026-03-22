---
phase: 10-site-reframe-plugin-first
verified: 2026-03-22T12:00:00Z
status: passed
score: 7/7 must-haves verified
gaps: []
accepted_as_designed:
  - truth: "Desktop nav, mobile overlay, and footer nav all show 'Vision' and 'Roadmap' instead of 'Hardware' and 'App'"
    resolution: "User accepted current design: App nav link removed intentionally (App section merged into Vision). No 'Roadmap' nav link needed. App section-label 'App' retained as-is. 2026-03-22."
human_verification:
  - test: "Scroll-spy highlight — Vision link"
    expected: "When scrolling through the hardware section, the 'Vision' desktop nav link highlights correctly"
    why_human: "Scroll-spy behavior requires runtime browser testing"
  - test: "Full visual pass — hero, hardware, shop"
    expected: "Hero shows single magenta 'Get Plugin' button. Hardware section has no price visible anywhere. Shop shows one card plus the 'MORE PRODUCTS IN DEVELOPMENT' note."
    why_human: "Visual rendering requires browser"
---

# Phase 10: Site Reframe — Plugin First Verification Report

**Phase Goal:** Reframe the site so the plugin is the primary product. Hardware and App become vision/roadmap sections — no pricing, no CTAs, no deposit.
**Verified:** 2026-03-22
**Status:** gaps_found
**Re-verification:** No — initial verification

---

## Goal Achievement

### Observable Truths

| #   | Truth | Status | Evidence |
| --- | ----- | ------ | -------- |
| 1   | Hero shows plugin as the primary CTA — no hardware preorder button visible | ✓ VERIFIED | Line 952: `<a href="#pageShop" class="btn-primary">Get Plugin — CHF 59 ...`. Hero-actions has exactly two children: btn-primary + btn-ghost-text. |
| 2   | No CHF 499 price anywhere on the page | ✓ VERIFIED | `grep "499" index.html` returns zero matches. All CHF references are plugin price CHF 59 / CHF 79. |
| 3   | Hardware section reads as in-development vision, no purchase CTA | ✓ VERIFIED | Status bar line 1208: "IN DEVELOPMENT". Status tag line 1210: "Daisy Seed · Pure Data · Concept & Prototype". Target price spec row absent. Roadmap timeline step (line 1398) has no CHF 499. section-label line 1159: "Vision". |
| 4   | Shop shows only the plugin card, with a 'More coming' note below it | ✓ VERIFIED | `grep "shop-card--preorder\|shop-card--dim" index.html` returns only a CSS rule (line 285), no DOM instances. Plugin card present (line 1628). shop-more-coming element present (line 1670): "More products in development — hardware and app coming in 2026." shop-desc updated (line 1622). |
| 5   | Desktop nav, mobile overlay, and footer nav all show 'Vision' and 'Roadmap' instead of 'Hardware' and 'App' | ✗ FAILED | 'Vision' correctly applied in all three locations. 'Roadmap' (App rename) absent from all three nav locations. App section-label (line 1435) still reads "App" not "Roadmap". |
| 6   | hw-waitlist form still present and intact | ✓ VERIFIED | Lines 1405–1422: hw-waitlist div with form, email input, and hwConfirm element all present. |
| 7   | navHardware scroll-spy ID preserved — scroll-spy still works | ✓ VERIFIED | `grep -c "id=\"navHardware\""` = 1. Scroll-spy JS references it at line 1801: `{id:'pageHardware', nav:'navHardware'}`. |

**Score:** 6/7 truths verified

---

### Required Artifacts

| Artifact | Expected | Status | Details |
| -------- | -------- | ------ | ------- |
| `index.html` | Entire reframed site — all changes in-place | ✓ VERIFIED | File exists. Contains `btn-primary.*Get Plugin` at line 952. All major changes confirmed by targeted grep. |

---

### Key Link Verification

| From | To | Via | Status | Details |
| ---- | -- | --- | ------ | ------- |
| hero-actions div | #pageShop | btn-primary anchor href | ✓ WIRED | Line 952: `href="#pageShop" class="btn-primary"` |
| navHardware anchor | #pageHardware | id=navHardware preserved, only text changed | ✓ WIRED | Line 927: `<a href="#pageHardware" id="navHardware">Vision</a>` — ID intact, text changed |

---

### Requirements Coverage

No requirement IDs were declared for this phase.

---

### Anti-Patterns Found

| File | Line | Pattern | Severity | Impact |
| ---- | ---- | ------- | -------- | ------ |
| index.html | 1435 | `section-label` reads "App" | ⚠️ Warning | App section still self-identifies as "App" in its section label, inconsistent with goal of renaming App to Roadmap |
| index.html | 285 | `.shop-card--dim` CSS rule remains | ℹ️ Info | Unused CSS from removed bundle card — dead code, no functional impact |

---

### Human Verification Required

#### 1. Scroll-spy highlight — Vision link

**Test:** Open the site in a browser and scroll through the hardware/vision section.
**Expected:** The "Vision" desktop nav link highlights (gets the active/teal colour) when the hardware section is in view.
**Why human:** Scroll-spy behaviour requires runtime browser testing.

#### 2. Full visual pass — hero, hardware, shop

**Test:** Open the site in a browser and inspect all three key sections.
**Expected:** Hero shows single magenta "Get Plugin — CHF 59" button with no hardware preorder button. Hardware section shows no price anywhere. Shop shows one card plus the small monospaced "MORE PRODUCTS IN DEVELOPMENT" note below it.
**Why human:** Visual rendering and layout require a browser.

---

### Gaps Summary

**One gap blocks full goal achievement.**

The plan's Task 2 called for renaming the "App" nav label to "Roadmap" in all three nav locations (desktop, mobile overlay, footer) and renaming the App `section-label` to "Roadmap". The "Vision" half of this task was completed correctly. The "Roadmap" half was not applied:

- Desktop nav (line 924–930): No App/Roadmap link present at all. The plan's listed 5-link desktop nav included an App entry — it was either removed or never existed as a standalone `<div class="page" id="pageApp">`. The App content lives as an inline section within the hardware page area, not as its own scrollable page.
- Mobile overlay (lines 4072–4077): No App/Roadmap link.
- Footer nav (lines 1697–1701): No App/Roadmap link.
- App section-label (line 1435): Still reads "App".

The SUMMARY documents this as a deviation note: "App section label left as 'App' internally; nav label renamed to 'Roadmap' then 'Vision' in follow-on commits." However, no follow-on commit appears to have applied the App section-label rename, and the nav link itself is absent rather than renamed.

This is a partial gap — the site is substantially plugin-first and the major removals (hardware price, preorder CTA, shop cards) are all complete. The remaining gap is a copy/label inconsistency in the App section that contradicts the goal framing.

---

_Verified: 2026-03-22_
_Verifier: Claude (gsd-verifier)_
