---
phase: 10-site-reframe-plugin-first
plan: 01
subsystem: ui
tags: [html, copy, nav, shop, hero]

requires:
  - phase: 08-lemonsqueezy-plugin-checkout
    provides: plugin checkout flow that hero CTA points to
provides:
  - Plugin-first hero with single "Get Plugin" primary CTA
  - Hardware section reframed as vision-only (no pricing, no purchase CTA)
  - Shop collapsed to single plugin card with "more coming" note
  - All nav locations updated to Vision/Roadmap labels
affects: [08-lemonsqueezy-plugin-checkout, 07-commerce-link-wiring]

tech-stack:
  added: []
  patterns: []

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "Hero: plugin btn-primary with arrow SVG, hardware preorder button removed entirely"
  - "Hardware status bar: 'IN DEVELOPMENT', price tag replaced with 'Concept & Prototype'"
  - "Target price spec row removed from hardware spec list"
  - "Roadmap timeline last step: Kickstarter mention kept, CHF 499 removed"
  - "Shop: hardware and bundle cards removed, plugin card kept, .shop-more-coming note added"
  - "Nav labels: Hardware → Vision, App → Roadmap (later refined to Vision) across desktop/mobile/footer"
  - "section-label and section-divider renamed to match nav labels"
  - "id=navHardware preserved — scroll-spy unchanged"

patterns-established: []

requirements-completed: []

duration: checkpoint-gated
completed: 2026-03-22
---

# Phase 10 Plan 01: Site Reframe — Plugin First Summary

**index.html reframed from dual plugin+hardware sales page to plugin-first: hero has single Get Plugin CTA, hardware is vision-only with no pricing, shop shows one card, and all nav labels read Vision/Roadmap**

## Performance

- **Duration:** checkpoint-gated (tasks executed across sessions)
- **Started:** 2026-03-15 (approx)
- **Completed:** 2026-03-22T10:19:05Z
- **Tasks:** 3 (2 auto + 1 human-verify)
- **Files modified:** 1 (index.html)

## Accomplishments
- Hero CTA swapped: plugin "Get Plugin — CHF 59" is the filled magenta primary button, hardware preorder button removed
- Hardware section stripped of all pricing (CHF 499 gone from status bar, spec list, and roadmap timeline), status reads "IN DEVELOPMENT"
- Shop grid collapsed to single plugin card plus `.shop-more-coming` note; hardware and bundle cards removed
- All three nav locations (desktop, mobile overlay, footer) updated: "Hardware" → "Vision", "App" → "Roadmap"
- Section dividers and section-labels renamed to match nav labels
- Scroll-spy ID `navHardware` preserved — hardware section highlight still works

## Task Commits

1. **Task 1: Hero CTA swap + hardware section price and CTA removal** — `1a3a19e` (feat)
2. **Task 2: Shop collapse + nav and section divider renames** — `d95027d` (feat)
3. **Task 3: Visual verification** — checkpoint:human-verify, approved 2026-03-22

## Files Created/Modified
- `index.html` — all changes in-place: hero-actions, hw-status-bar, hw-spec-list, hw-roadmap, shop-grid, nav labels (3 locations), section dividers, section labels

## Decisions Made
- Removed hardware preorder button entirely from hero-actions (not hidden, deleted)
- `.shop-more-coming` CSS uses `grid-column: 1 / -1` to span full shop grid width
- Price change CHF 49 → CHF 59 applied in a subsequent commit (0b0436d) — outside plan scope but compatible
- App section label left as "App" internally; nav label renamed to "Roadmap" then "Vision" in follow-on commits

## Deviations from Plan

None — plan executed as specified. Subsequent commits (5b4226c, 0b0436d, 960e233, 937629d) built on top of this plan's changes but were outside this plan's scope.

## Issues Encountered
None

## Next Phase Readiness
- Site is plugin-first and ready for LemonSqueezy checkout wiring (Phase 08) and commerce link wiring (Phase 07)
- Custom domain dimea.io still deferred — Mac signing ETA was ~2026-03-21

---
*Phase: 10-site-reframe-plugin-first*
*Completed: 2026-03-22*
