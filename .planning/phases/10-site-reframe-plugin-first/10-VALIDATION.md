---
phase: 10
slug: site-reframe-plugin-first
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-03-15
---

# Phase 10 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | None — static flat HTML, no test runner |
| **Config file** | none |
| **Quick run command** | `grep -n "499\|[Pp]reorder" index.html` — expect 0 matches |
| **Full suite command** | `grep -n "499\|[Pp]reorder\|hardware pre-order" index.html` — expect 0 matches |
| **Estimated runtime** | ~1 second |

---

## Sampling Rate

- **After every task commit:** Run `grep -n "499\|[Pp]reorder" index.html`
- **After every plan wave:** Run full grep suite + manual browser inspection
- **Before `/gsd:verify-work`:** Full grep must return 0 matches; manual browser pass complete
- **Max feedback latency:** ~5 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Behaviour | Test Type | Automated Command | Status |
|---------|------|------|-----------|-----------|-------------------|--------|
| 10-01-01 | 01 | 1 | Hero: plugin is btn-primary, hardware button gone | grep + manual | `grep -n "Preorder Hardware" index.html` → 0 | ⬜ pending |
| 10-01-02 | 01 | 2 | Hardware section: no CHF 499, no purchase CTA | grep | `grep -n "499" index.html` → 0 | ⬜ pending |
| 10-01-03 | 01 | 3 | Shop: single card, "More coming" note present | grep + manual | `grep -n "shop-card--preorder\|shop-card--dim" index.html` → 0 | ⬜ pending |
| 10-01-04 | 01 | 4 | Nav: "Vision" and "Roadmap" labels, no "Hardware"/"App" text | grep | `grep -c "navHardware" index.html` → 1 (id preserved) | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

None — no test infrastructure exists or is needed. Static HTML content edit only. Grep is the automated verification layer.

*Existing infrastructure covers all phase requirements.*

---

## Manual-Only Verifications

| Behavior | Why Manual | Test Instructions |
|----------|------------|-------------------|
| Plugin CTA is visually primary (magenta/filled) in hero | CSS class swap — need visual confirmation | Open site in browser, scroll to hero — plugin button should be filled/magenta, no hardware button present |
| Hardware section has no purchase CTA visible | DOM inspection | Scroll to #pageHardware — no "Buy", "Order", "Deposit" button visible anywhere in section |
| App section has vision/development framing | Content tone check | Scroll to #pageApp — no purchase CTA, badge reads "In Development" or similar |
| Shop shows only plugin card (no hardware, no bundle) | Grid layout check | Scroll to #pageShop — single card visible, grid not stretching card full-width |
| "More coming" note visible below plugin card | DOM inspection | Below plugin shop card — small monospaced text note present |
| Nav shows "Vision" (not "Hardware") on desktop AND mobile | Three-location check | Desktop nav header, mobile hamburger overlay, footer nav — all show new labels |
| hw-waitlist form still present and functional | Phase 09 non-regression | Scroll to bottom of #pageHardware — email input present, submit button present |
| navHardware ID preserved (scroll-spy works) | JS non-regression | Scroll to hardware section — nav link highlights correctly |

---

## Validation Sign-Off

- [ ] All tasks have grep verify or manual verify steps
- [ ] Sampling continuity: every wave ends with grep check
- [ ] Wave 0: N/A (no test infrastructure needed)
- [ ] No watch-mode flags
- [ ] Feedback latency < 5s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
