---
phase: 3
slug: legal-pages
status: draft
nyquist_compliant: false
wave_0_complete: true
created: 2026-03-13
---

# Phase 03 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | none — plain HTML/CSS, no test runner |
| **Config file** | none |
| **Quick run command** | Manual browser inspection |
| **Full suite command** | Manual browser inspection of all three pages + footer |
| **Estimated runtime** | ~2 minutes |

---

## Sampling Rate

- **After every task commit:** Open changed file(s) in browser, run manual checklist items for that task
- **After every plan wave:** Full manual checklist across all deliverables
- **Before `/gsd:verify-work`:** All manual checks green
- **Max feedback latency:** N/A (manual only — no automated runner)

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 03-01-privacy | 01 | 1 | Privacy Policy complete | manual | open privacy.html in browser | ❌ pre-exists | ⬜ pending |
| 03-01-terms | 01 | 1 | Terms of Service complete | manual | open terms.html in browser | ❌ pre-exists | ⬜ pending |
| 03-02-refund | 02 | 2 | Refund Policy complete | manual | open refund.html in browser | ❌ pre-exists | ⬜ pending |
| 03-02-footer | 02 | 2 | Footer links wired | manual | click footer links in index.html | ✅ exists | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

None — this phase creates new HTML files. No test infrastructure is required or appropriate for static content pages. Existing infrastructure covers all phase requirements (there is no test infrastructure to set up).

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| White/light background on all legal pages | User decision: light theme | No test runner in project | Open privacy.html, terms.html, refund.html in browser; confirm no dark background |
| Back link navigates to index.html | User decision: minimal chrome | No test runner | Click "← Back to dimea.audio" on each legal page |
| All four third-party processors listed | Privacy policy requirement | Content verification | Read privacy.html processors section; confirm LemonSqueezy, Stripe, Mailchimp, Coinbase Commerce all present |
| Hardware cancellation clause present | Refund policy requirement | Content verification | Search refund.html for "Dimea Arcade cancels" or equivalent |
| License revocation clause present | Refund policy requirement | Content verification | Search refund.html for "license" and "revoked" or "deactivated" |
| Footer has Privacy, Terms, Refund, Contact links | Footer deliverable | DOM inspection | Check `.footer-legal` in browser; all four links clickable and non-placeholder |
| No `href="#"` placeholders remain in footer | Footer deliverable | DOM inspection | Inspect `.footer-legal` anchors; none should navigate to top of page |
| Fonts load (Syne headings + Space Grotesk body) | Visual design decision | Network tab check | Open browser devtools Network tab; confirm fonts.googleapis.com requests succeed |
| Impressum/Data Controller section in privacy.html | Legal requirement | Content verification | Search privacy.html for "Dimea Arcade, operated by Dimitri Dähler" |
| Plausible placeholder section in privacy.html | User decision | Content verification | Search privacy.html for "Plausible" |

---

## Validation Sign-Off

- [ ] All tasks have manual verification steps defined above
- [ ] No automated tests needed (project has no test runner — plain HTML/CSS)
- [ ] Wave 0: not applicable (no test stubs to create)
- [ ] All manual checks executable with browser devtools only
- [ ] `nyquist_compliant: true` set in frontmatter when all checks pass

**Approval:** pending
