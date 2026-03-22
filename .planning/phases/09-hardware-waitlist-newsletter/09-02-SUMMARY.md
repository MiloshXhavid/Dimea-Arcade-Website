---
phase: 09-hardware-waitlist-newsletter
plan: 02
subsystem: email
tags: [mailchimp, jsonp, email, waitlist, newsletter, single-opt-in]

# Dependency graph
requires: [09-01]
provides:
  - "Mailchimp JSONP integration for hardware waitlist form"
  - "Mailchimp JSONP integration for footer newsletter form"
  - "Inline success/error UI for both email forms"
  - "Privacy compliance micro-copy with /privacy links on both forms"
affects: [index.html]

# Tech tracking
tech-stack:
  added: [mailchimp-jsonp-script-tag-injection]
  patterns:
    - "JSONP via dynamic script tag — only approach that works without CORS headers"
    - "Unique callback name using Date.now() suffix to prevent collision"
    - "Already-subscribed emails treated as success to avoid confusing errors"

key-files:
  created: []
  modified:
    - "index.html — hardware waitlist and footer newsletter forms wired to Mailchimp JSONP"

key-decisions:
  - "Single opt-in — confirmation text is '✓ You're on the list.' (not double opt-in 'Check your inbox')"
  - "No group params — single Mailchimp list, both forms submit email only"
  - "Honeypot field b_ffa1cafa22936c5b074e7f303_5a54dcd314 appended to JSONP URL as empty param to avoid bot filtering"
  - "submitToMailchimp takes (email, onSuccess, onError) — no groupParam"
  - "From email is contact@dimea.io"

# Metrics
duration: 2min
completed: 2026-03-22
---

# Phase 09 Plan 02: Hardware Waitlist + Newsletter — JSONP Integration Summary

**Mailchimp JSONP wired to both email forms with single opt-in, inline confirm/error UI, and privacy compliance notes**

## Performance

- **Duration:** ~2 min
- **Started:** 2026-03-22T00:56:59Z
- **Completed:** 2026-03-22T00:59:12Z
- **Tasks:** 2/3 auto complete (Task 3 is checkpoint:human-verify — awaiting approval)
- **Files modified:** 1 (index.html)

## Accomplishments

- Hardware waitlist form: JSONP submit to Mailchimp, inline "✓ You're on the list." confirmation, inline error display, double-submit prevention
- Footer newsletter form: same JSONP pattern, separate confirm/error elements, button wired with onclick handler
- Privacy Policy links added to both forms pointing to /privacy
- Error elements added to DOM (hidden by default) for both forms
- Honeypot bot-protection field included in JSONP URL
- Already-subscribed emails treated as success (no confusing error for returning subscribers)

## Task Commits

1. **Task 1: HTML edits** — `228f310` — error elements, privacy notes, footer button wired, confirmation text updated
2. **Task 2: JS implementation** — `cc36d11` — submitToMailchimp JSONP helper, handleHwWaitlist() replaced, handleFooterNewsletter() added

## Files Created/Modified

- `index.html` — hardware waitlist JSONP + footer newsletter JSONP + privacy notes + error elements

---

## Deviations from Plan

### Applied Objective Overrides

**1. Single opt-in confirmation text**
- **Plan said:** "✓ Check your inbox — click the link to confirm your spot."
- **Applied instead:** "✓ You're on the list." (per objective — single opt-in, not double)
- **Reason:** User explicitly mandated single opt-in with this exact confirmation text

**2. No group params**
- **Plan said:** HW_GROUP_PARAM and NL_GROUP_PARAM appended to JSONP URL
- **Applied instead:** submitToMailchimp takes (email, onSuccess, onError) — no groupParam at all
- **Reason:** Single Mailchimp list, user chose not to use groups

**3. Honeypot in JSONP URL**
- **Plan said:** Not explicitly mentioned for JSONP URL
- **Applied:** `&b_ffa1cafa22936c5b074e7f303_5a54dcd314=` appended to JSONP URL
- **Reason:** User explicitly requested this to avoid bot filtering

**4. From email**
- **Plan/09-01 said:** contact@dimea.audio
- **Applied:** contact@dimea.io (per objective override)

## Checkpoint Status

Task 3 (checkpoint:human-verify) is pending. Awaiting live smoke test of both forms.

## Self-Check: PASSED

- `index.html` modified — confirmed via git log (228f310, cc36d11)
- No duplicate handleHwWaitlist() definitions
- CURSOR block follows END MAILCHIMP block correctly
- Both privacy links point to /privacy
- footerConfirm, footerError, hwError all exist in DOM
