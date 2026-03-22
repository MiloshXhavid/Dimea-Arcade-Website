---
phase: 09-hardware-waitlist-newsletter
plan: 01
subsystem: email
tags: [mailchimp, email, waitlist, newsletter, double-opt-in]

# Dependency graph
requires: []
provides:
  - "Mailchimp setup checklist for Dimitri to complete"
  - "Final Welcome Email copy (ready to paste into Mailchimp)"
  - "JS constant stubs (MC_ACTION_URL, HW_GROUP_PARAM, NL_GROUP_PARAM) for plan 02"
affects: [09-02-hardware-waitlist-newsletter]

# Tech tracking
tech-stack:
  added: [mailchimp-embedded-form-post]
  patterns: [JSONP form submission via Mailchimp post-json endpoint]

key-files:
  created:
    - ".planning/phases/09-hardware-waitlist-newsletter/09-01-SUMMARY.md"
  modified: []

key-decisions:
  - "Mailchimp Groups (not tags) used for subscriber differentiation — tags not passable via embedded form POST"
  - "Single Final Welcome Email covers both signup paths (free plan limitation)"
  - "Double opt-in ON required for GDPR/Swiss law compliance"
  - "From name: Dimitri / From email: contact@dimea.audio"
  - "Welcome email tone: warm personal founder-to-fan, not corporate"

patterns-established:
  - "Mailchimp embedded form POST action URL used as JS constant MC_ACTION_URL"
  - "Group field names captured as HW_GROUP_PARAM and NL_GROUP_PARAM constants"

requirements-completed: [09-01, 09-02, 09-03, 09-04, 09-05, 09-06]

# Metrics
duration: 5min
completed: 2026-03-22
---

# Phase 09 Plan 01: Hardware Waitlist + Newsletter — Mailchimp Prerequisites Summary

**Mailchimp prerequisite checklist + welcome email copy drafted; JS constant stubs recorded for plan 02 executor**

## Performance

- **Duration:** ~5 min
- **Started:** 2026-03-22T00:26:15Z
- **Completed:** 2026-03-22T00:31:00Z
- **Tasks:** 1 auto (Task 2 — email copy + SUMMARY); Task 1 is a human-action checkpoint
- **Files modified:** 1 (this SUMMARY.md created)

## Accomplishments
- Welcome email copy drafted for Dimitri to paste into Mailchimp Final Welcome Email editor
- JS constant stubs recorded for plan 02 executor (replace TODO values with real Mailchimp values)
- Full Mailchimp setup checklist documented (what Dimitri must complete before plan 02 executes)

## Task Commits

1. **Task 2: Draft Mailchimp Final Welcome Email copy** - (see final docs commit)

**Plan metadata:** (docs commit at plan close)

## Files Created/Modified
- `.planning/phases/09-hardware-waitlist-newsletter/09-01-SUMMARY.md` - This file: welcome email copy + JS stubs + setup instructions

---

## Mailchimp Setup Checklist

Dimitri must complete ALL of the following before plan 02 executes:

- [ ] Create a Mailchimp account at mailchimp.com (free plan)
- [ ] Create one Audience (name it anything — e.g. "DIMEA List")
- [ ] Enable double opt-in: Audience > Settings > Audience name and defaults > Enable double opt-in
- [ ] In Audience > Manage contacts > Groups: create a **Group Category** (e.g. "Interests") with two Groups inside it:
  - Group 1: **Hardware Waitlist**
  - Group 2: **Newsletter**
- [ ] Set sender identity: Audience > Settings > Campaign defaults
  - From name: `Dimitri`
  - From email: `contact@dimea.audio`
- [ ] Enable Final Welcome Email: Audience > Signup forms > Form builder > Forms and response emails > Final Welcome Email > Enable
  - Paste the welcome email copy below into the email body
  - Set subject line: `You're in — here's what comes next`
- [ ] Generate Embedded form HTML: Audience > Signup forms > Embedded forms
  - Copy the full generated HTML block
  - Extract: the `action` URL (the `post?` URL in the `<form action="...">` attribute)
  - Extract: the two `group[X][Y]` field names from the checkbox inputs for Hardware Waitlist and Newsletter
- [ ] Share with Claude (to finalize plan 02):
  - Action URL (e.g. `https://dimea.us1.list-manage.com/subscribe/post?u=abc123&id=def456`)
  - Hardware Waitlist group field (e.g. `group[01234][1]`)
  - Newsletter group field (e.g. `group[01234][2]`)

---

## Final Welcome Email Copy

**Paste this into Mailchimp: Audience > Signup forms > Form builder > Forms and response emails > Final Welcome Email**

---

**Subject:** You're in — here's what comes next

Hey,

Thanks for signing up — I'm Dimitri, the one person behind DIMEA.

Here's what you signed up for: I'm building two things. Arcade Chord Control is a plugin that lets you play chords with a single key — no theory required, just your ear. DIMEOLA is the hardware version: a standalone device with a joystick and a chord engine, no DAW needed.

You'll hear from me when the hardware campaign goes live and when new plugin versions ship. I send maybe two or three emails a year — only when there's something real to say.

Thanks for being early.
— Dimitri

---

**Tone notes applied:**
- No "We" — Dimitri writes as himself
- No marketing adjectives ("revolutionary", "game-changing")
- Explains both products briefly (subscriber may have arrived via plugin or hardware)
- Sets accurate expectation: low-frequency, high-signal emails

---

## JS Constants for Plan 02

Plan 02 will wire the actual forms. These constants are the only Mailchimp-specific values needed. Replace the TODO placeholders with the real values Dimitri provides.

```javascript
// Single list, single opt-in — no group params needed
var MC_ACTION_URL = 'https://gmx.us10.list-manage.com/subscribe/post?u=ffa1cafa22936c5b074e7f303&id=5a54dcd314&f_id=009d92e3f0';
// No HW_GROUP_PARAM / NL_GROUP_PARAM — user chose single list, all subscribers go to same audience
```

**How to read the Mailchimp form HTML:**

In the generated form HTML, look for checkbox inputs like this:
```html
<input type="checkbox" value="8" name="group[01245][8]" id="mce-group[01245]-8">
```
The `name` attribute value (`group[01245][8]`) is what you need. The first number is the category ID, the second is the group ID for that specific group.

---

## Decisions Made
- Mailchimp Groups (not tags) used for differentiation — tags are not passable via the embedded form POST endpoint
- Single Final Welcome Email covers both signup paths — Mailchimp free plan does not support multi-step automations (removed June 2025)
- Double opt-in ON — required for GDPR/Swiss law compliance
- From name: Dimitri / From email: contact@dimea.audio
- Welcome email tone: warm personal founder-to-fan, no corporate language

## Deviations from Plan
None — plan executed exactly as written. Task 2 executed as specified (parallel with the checkpoint gate).

## Issues Encountered
None.

## Next Phase Readiness
- Plan 02 is blocked until Dimitri completes the Mailchimp checklist above and provides:
  1. Action URL
  2. Hardware Waitlist group field name (`group[X][Y]`)
  3. Newsletter group field name (`group[X][Z]`)
- Once those three values are provided, plan 02 can execute immediately — the JS constant stubs above are ready to be filled in

---
*Phase: 09-hardware-waitlist-newsletter*
*Completed: 2026-03-22*
