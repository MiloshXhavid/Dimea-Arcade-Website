---
gsd_state_version: 1.0
milestone: v1.0
milestone_name: milestone
status: unknown
last_updated: "2026-03-13T19:44:22.472Z"
---

## Position

Phase 03 in progress. Plan 03-01 complete (privacy.html + terms.html).

## Decisions

- Audio race condition fixed: `audioCtx.resume()` awaited before `triggerChord` fires
- Mobile nav uses `[ MENU ]` full-screen overlay toggle at <= 900px
- Touch devices get `cursor:auto` and hidden cursor ring via `@media (pointer: coarse)`
- Plugin screenshot embedded from assets/arcade-chord-control.png
- Video embed skipped (awaiting YouTube/Vimeo URL from user)
- Plugin title bar implemented as real DOM `.plugin-ui-frame-bar` div (removed ::before approach)
- Manifesto quote updated to "Harmony shouldn't live behind a theory wall." after human review (02-01)
- About bio grid uses 3fr 2fr column ratio; portrait aspect-ratio 4/5 with object-position center top (02-01)
- Founder intro inserted between .hw-header and .hw-origin — above the 3-step strip, not inside it
- Font size uses clamp(18px, 2vw, 24px) on .hw-founder-voice for fluid scaling
- Image paths corrected to relative assets/ — matches existing working pattern
- Impressum folded into privacy.html under "About this Site / Data Controller" — no separate impressum.html (03-01)
- Legal pages use light theme (white bg, dark text) — distinct from main site dark navy brand (03-01)
- Identical CSS block per file — consistent with flat HTML project, no shared stylesheet (03-01)
- Plausible Analytics added as placeholder section in privacy.html — pending activation (03-01)

## Commits (Phase 01)

- `949d22f` init: DIMEA website v2 with joystick audio + arcade easter egg
- `8bdf67c` fix: audio engine + rename plugin to Arcade Chord Control
- `e64214a` feat: mobile responsive layout (sub-plan 01-02)
- `00e1561` feat(01-03): embed plugin screenshot and add video placeholder

## Commits (Phase 02)

- `d83f28d` docs(02): create phase 02 plan — about/story/vision
- `a5719ef` chore(02-01): copy founder portrait to assets
- `14fd663` feat(02-01): add #pageAbout section with manifesto, bio, and nav links
- `6726e75` fix(02): update manifesto quote, gate square wave to upper half, swap plugin screenshot
- `0554550` feat(02-02): add hardware origin images to assets
- `ca50398` feat(02-02): insert hw-founder-intro paragraph and fix broken image paths

## Commits (Phase 03)

- `225cbf3` feat(03-01): create privacy.html — Privacy Policy + Impressum
- `07e1e3e` feat(03-01): create terms.html — Terms of Service

## Next

Phase 03 plan 03-02: refund.html + footer link wiring in index.html
