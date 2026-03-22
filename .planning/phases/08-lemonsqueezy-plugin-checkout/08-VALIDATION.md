---
phase: 8
slug: lemonsqueezy-plugin-checkout
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-03-14
---

# Phase 8 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual browser smoke testing — no automated test framework |
| **Config file** | none |
| **Quick run command** | Open `index.html` in browser, click "Buy Now" in plugin section |
| **Full suite command** | Verify all 3 buy-button touch-points open LS overlay; verify "Try Free" links; verify no console errors |
| **Estimated runtime** | ~2 minutes |

---

## Sampling Rate

- **After every task commit:** Open index.html locally, verify no console errors
- **After every plan wave:** Run full browser smoke test across all 3 buy-button touch-points
- **Before `/gsd:verify-work`:** Full suite must be green (all 6 manual checks pass)
- **Max feedback latency:** ~120 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 08-01-01 | 01 | 1 | 08-01 | manual smoke | Open index.html → click plugin section Buy Now → LS overlay opens | ✅ | ⬜ pending |
| 08-01-02 | 01 | 1 | 08-02 | manual smoke | Click shop section Buy Now → LS overlay opens | ✅ | ⬜ pending |
| 08-01-03 | 01 | 1 | 08-03 | manual smoke | Scroll page → click sticky CTA → LS overlay opens | ✅ | ⬜ pending |
| 08-01-04 | 01 | 1 | 08-04 | manual smoke | Confirm `.shop-checkout` email form is gone from HTML | ✅ | ⬜ pending |
| 08-01-05 | 01 | 1 | 08-05 | manual smoke | Click "Try Free" → verify href is set (placeholder or real URL) | ✅ | ⬜ pending |
| 08-01-06 | 01 | 1 | 08-06 | manual smoke | Open DevTools → load page → no `LemonSqueezy is not defined` errors | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- No Wave 0 needed — this phase has no automated test framework. All validation is manual browser smoke testing.
- LS test mode: Use LS sandbox checkout URL during development to avoid real charges.

*Existing infrastructure covers all phase requirements (manual-only).*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| LS overlay opens on Buy Now click (plugin section) | 08-01 | Requires browser + live LS CDN script | Load index.html → click Buy Now in plugin section → overlay modal appears |
| LS overlay opens on Buy Now click (shop section) | 08-02 | Requires browser + live LS CDN script | Click Buy Now in shop/pricing section → overlay modal appears |
| LS overlay opens from sticky CTA | 08-03 | Requires browser + JS execution | Scroll down → click sticky "Buy Now →" button → overlay modal appears |
| Email-collection form is gone | 08-04 | Visual/structural check | Inspect DOM — `.shop-checkout` block must not exist in rendered HTML |
| Try Free button links correctly | 08-05 | Visual/href check | Right-click Try Free → inspect href (should not be `#` if real URL available) |
| No JS console errors on load | 08-06 | DevTools console check | Open DevTools → load page → zero red errors in Console tab |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 120s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
