---
gsd_state_version: 1.0
milestone: v1.0
milestone_name: milestone
status: unknown
last_updated: "2026-03-22T10:39:42.261Z"
progress:
  total_phases: 21
  completed_phases: 8
  total_plans: 12
  completed_plans: 11
---

## Position

Phase 05 plan 01 complete. Branch renamed to main, .gitignore and vercel.json committed and pushed to GitHub (MiloshXhavid/Dimea-Arcade-Website). Vercel project connected — site live at dimea-arcade-website-cfy8wg6sg-miloshxhavids-projects.vercel.app (HTTP 200, clean URLs working). Phase 05 plan 02 (custom domain dimea.io) deferred — waiting for plugin Mac signing (~2026-03-21) and commerce setup before purchasing domain.

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
- Cloudflare-obfuscated Contact href replaced with plain mailto:contact@dimea.audio — Vercel static hosting has no CDN email protection (03-02)
- Hardware deposit non-refundable notice uses &lt;p&gt;&lt;strong&gt; for prominence — no additional CSS needed (03-02)
- Dimea Arcade cancellation clause included in refund.html — full deposit refund within 30 days if DIMEOLA project cancelled (03-02)
- [Phase 04-seo-meta-favicon]: og:image reused across legal pages — branded fallback, no page-specific art exists
- [Phase 04-seo-meta-favicon]: manual.html excluded from sitemap and given noindex — internal reference doc, not product page
- [Phase 04-seo-meta-favicon]: JSON-LD placed as last element inside <head> before </head> — after all inline CSS
- [Phase 04-02]: OG image redesigned to oscilloscope visual + DIMEA wordmark — stronger social sharing impact than text-only card
- [Phase 04-02]: Icon SVG redesigned with smooth arc, oval dots, and DIMEA text label for legibility at favicon sizes
- [Phase 04-02]: apple-touch-icon.png uses filled dark background — transparent backgrounds render black on iOS home screen
- [Phase 05-domain-vercel-deploy]: vercel.json uses explicit rewrites (not wildcard) for 5 SPA routes — legal pages served as real files
- [Phase 05-domain-vercel-deploy]: Vercel project connected to MiloshXhavid/Dimea-Arcade-Website, production branch main, framework Other, output dir .
- [Phase 05-domain-vercel-deploy]: Custom domain dimea.io deferred — purchase after plugin Mac signing + commerce setup (~2026-03-21)
- [Phase 09-hardware-waitlist-newsletter]: Mailchimp Groups (not tags) for subscriber differentiation — tags not passable via embedded form POST
- [Phase 09-hardware-waitlist-newsletter]: Single Final Welcome Email covers both signup paths — Mailchimp free plan removed multi-step automations June 2025
- [Phase 09-02]: Single opt-in — confirmation text "✓ You're on the list." — no double opt-in email flow
- [Phase 09-02]: No group params — single Mailchimp list, both forms submit email only via JSONP
- [Phase 09-02]: Honeypot field b_ffa1cafa22936c5b074e7f303_5a54dcd314 appended to JSONP URL as empty param for bot protection
- [Phase 09-02]: Single opt-in — confirmation text '✓ You're on the list.' (not double opt-in)
- [Phase 09-02]: No group params — single Mailchimp list, both forms submit email only
- [Phase 09-02]: Honeypot field appended to JSONP URL for bot protection

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
- `7cd990c` feat(03-02): create refund.html — Refund Policy
- `a70ee3c` feat(03-02): wire footer legal links in index.html

## Commits (Phase 04)

- `089f91a` feat(04-01): add full SEO head to index.html — title, OG, Twitter, JSON-LD, favicon
- `e7299b3` feat(04-01): add SEO meta to legal pages, noindex to manual, create sitemap + robots
- `c154a07` feat(04-02): create favicon files — icon.svg, favicon.ico, apple-touch-icon.png
- `5c679dd` feat(04-02): redesign logo mark — smooth arc, oval dots, DIMEA text in icon
- `7953256` feat(04-02): add generated OG social preview image (1200x630)
- `7f6949f` feat(og-image): redesign social preview — oscilloscopes + DIMEA wordmark

## Commits (Phase 05)

- `1cf8d4f` chore(05-01): add .gitignore — OS/editor exclusions only
- `d340bd6` feat(05-01): add vercel.json — clean URL rewrites for SPA sections

## Next

Phase 09 complete. Both email forms (hardware waitlist + footer newsletter) live and verified — JSONP wired to Mailchimp, single opt-in, inline confirm/error UI, privacy notes. Commits: 228f310 (HTML), cc36d11 (JS). Summary at .planning/phases/09-hardware-waitlist-newsletter/09-02-SUMMARY.md.

Phase 10 Plan 01 paused at Task 3 (checkpoint:human-verify). Tasks 1 and 2 committed (1a3a19e, d95027d). User must verify the reframed site in browser — then resume to complete plan and create SUMMARY. Once Phase 10-01 is approved: proceed to Phase 10-02 if planned, otherwise Phase 07 (Commerce). Custom domain dimea.io still deferred pending Mac signing (~2026-03-21).
