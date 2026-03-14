# Dimea Arcade Website — Roadmap

**Milestone:** v1.0 — Launch-Ready Site
**Goal:** A live, professional website with working plugin sales and hardware waitlist. Ships before or with plugin launch (~2 weeks).

---

## Phase 01: Fix, Polish & Mobile

**Goal:** Take the existing index.html from prototype to production-ready. Fix all console errors, restore joystick audio, make it work on mobile, and ensure every section is complete.

**Delivers:**
- Zero JS console errors
- Joystick XY pad plays audio on click (Web Audio engine working)
- Mobile-responsive layout (nav, hero, all sections)
- Arcade easter egg game launches on "Explore Free" button
- Plugin screenshot embedded in plugin section
- Rough demo video embedded (IMG_7305.mov) with "rough demo" label
- All nav links working, smooth scroll
- Status bar, custom cursor, all animations intact

**Status:** TODO

Plans:
- [ ] 01-01: Diagnose and fix all console errors + joystick audio
- [ ] 01-02: Mobile layout + responsive fixes
- [ ] 01-03: Media embeds (video + screenshot)

---

## Phase 02: About / Story / Vision

**Goal:** Add the founder story, manifesto, and brand vision as a dedicated section. This is the emotional core of the site — it sells the person, not just the product.

**Delivers:**
- Manifesto section: "Most musicians spend years learning theory just to play a chord. We think that's backwards."
- Founder bio: Dimitri Dähler — jazz trumpet, HSLU, modular rig, masterproject performance
- Hardware origin story: "This didn't start as a product. It started as a problem."
- Founder photo placeholder (ready for image drop-in)
- Visual design matching brand (navy/teal/magenta)

**Status:** TODO

Plans:
- [ ] 02-01: About / Manifesto section HTML + CSS
- [ ] 02-02: Hardware origin story section (within hardware page)

---

## Phase 03: Legal Pages

**Goal:** Create the three pages required by LemonSqueezy and Stripe before payments go live.

**Delivers:**
- `/privacy.html` — Privacy Policy (GDPR-compliant)
- `/terms.html` — Terms of Service
- `/refund.html` — Refund Policy (no-questions 14-day for plugin, deposit non-refundable for hardware with clear notice)
- Footer links to all three
- Cookie-free analytics note (Plausible = no consent banner needed)

**Status:** In progress — 1/2 plans complete

Plans:
- [x] 03-01-PLAN.md — Privacy Policy (privacy.html) + Terms of Service (terms.html)
- [ ] 03-02-PLAN.md — Refund Policy (refund.html) + footer links wired in index.html

---

## Phase 04: SEO, Meta & Favicon

**Goal:** Make the site look good when shared, rank for relevant searches, and have a proper favicon.

**Delivers:**
- `<title>`, meta description, canonical URL
- Open Graph tags (og:title, og:description, og:image) — link previews on Discord/Twitter
- OG image: 1200×630px branded card (static PNG)
- Favicon: favicon.ico (32×32 ICO), icon.svg (scalable, dark-mode aware), apple-touch-icon.png (180×180)
- sitemap.xml
- robots.txt
- Structured data (JSON-LD: SoftwareApplication for plugin)

**Status:** Complete — 2/2 plans executed

Plans:
- [x] 04-01-PLAN.md — Meta tags + OG tags + sitemap + robots.txt (all 5 HTML files + 2 new static files)
- [x] 04-02-PLAN.md — Favicon files (icon.svg, favicon.ico, apple-touch-icon.png) + OG image (human checkpoint)

---

## Phase 05: Domain + Vercel Deploy

**Goal:** Get the site live on a real domain.

**Delivers:**
- Vercel project connected to dimea-website git repo
- Custom domain configured (dimea.audio or dimea.io)
- HTTPS automatic
- Deploy preview URLs on every git push
- `.gitignore` + repo clean

**Status:** TODO

Plans:
- [ ] 05-01: Vercel setup + git remote + first deploy
- [ ] 05-02: Domain purchase + DNS configuration

---

## Phase 06: Joystick Audio — Warm Analog Sound

**Goal:** Transform the joystick XY pad from a digital-sounding raw oscillator engine into a warm, lush, analog-feeling synth pad — inspired by the Elka Synthex and Oberheim OB-8.

**Delivers:**
- Soft waveforms only (sine + triangle) — sawtooth and square removed from random pool
- Studio plate reverb with medium presence — old tail fades on chord change
- ~200ms delay, darker filtered repeats, 3 decaying repeats, always on
- Soft attack (~30ms) preserving punchiness
- 1s release tail on chord lift
- Filter stays dark overall — sweep range reduced, never fully opens
- Sub-oscillator retained

**Status:** TODO

Plans:
- [ ] 06-01: Waveform + filter + envelope reshape
- [ ] 06-02: Plate reverb implementation (Web Audio convolver or algorithmic)
- [ ] 06-03: Delay engine (filtered feedback loop, 3 repeats)

---

*Milestone goal: Professional live website ready for plugin launch*
*Created: 2026-03-13*
