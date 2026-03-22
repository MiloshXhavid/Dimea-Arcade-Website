---
phase: 9
slug: hardware-waitlist-newsletter
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-03-14
---

# Phase 9 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual browser smoke testing — no automated test framework |
| **Config file** | none |
| **Quick run command** | Open index.html → submit waitlist form with test email → check network request fires |
| **Full suite command** | Verify both forms submit JSONP request, inline confirmations appear, privacy links work, error states show |
| **Estimated runtime** | ~3 minutes |

---

## Sampling Rate

- **After every task commit:** Open index.html locally, verify no console errors on load
- **After every plan wave:** Run full browser smoke test on both forms
- **Before `/gsd:verify-work`:** Full suite must be green (all manual checks pass)
- **Max feedback latency:** ~180 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 09-01-01 | 01 | 1 | 09-01 | manual smoke | Submit waitlist form → network tab shows POST to Mailchimp endpoint | ✅ | ⬜ pending |
| 09-01-02 | 01 | 1 | 09-02 | manual smoke | Submit footer form → network tab shows POST to Mailchimp endpoint | ✅ | ⬜ pending |
| 09-01-03 | 01 | 1 | 09-03 | manual smoke | Submit invalid email → inline error message appears | ✅ | ⬜ pending |
| 09-01-04 | 01 | 1 | 09-04 | manual smoke | Success → confirmation text shows "Check your inbox — click the link to confirm" | ✅ | ⬜ pending |
| 09-01-05 | 01 | 1 | 09-05 | manual smoke | Privacy note "By subscribing..." visible on both forms with working link to /privacy | ✅ | ⬜ pending |
| 09-01-06 | 01 | 1 | 09-06 | manual smoke | No console errors on page load | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- No Wave 0 needed — manual browser smoke testing only. No test framework to install.

*Existing infrastructure covers all phase requirements (manual-only).*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Waitlist form submits to Mailchimp | 09-01 | Requires Mailchimp account + live endpoint | Submit form, open DevTools Network tab, verify JSONP script tag fires to list-manage.com |
| Footer form submits to Mailchimp | 09-02 | Requires live endpoint | Same as above for footer form |
| Invalid email shows error | 09-03 | Visual check | Enter "notanemail", click submit, verify error message appears below form |
| Success text updated for double opt-in | 09-04 | Visual check | Enter valid email, verify "Check your inbox" message (not old "You're on the list") |
| Privacy note links to /privacy | 09-05 | Visual + navigation check | Click privacy link, verify privacy.html loads |
| No console errors | 09-06 | DevTools check | Open DevTools → Console tab → load page → zero red errors |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 180s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
