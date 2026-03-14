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

**Status:** DONE

Plans:
- [x] 01-01: Fix, polish & mobile (audio engine, responsive layout, embeds)

---

## Phase 02: About / Story / Vision

**Goal:** Add the founder story, manifesto, and brand vision as a dedicated section. This is the emotional core of the site — it sells the person, not just the product.

**Delivers:**
- Manifesto section: "Most musicians spend years learning theory just to play a chord. We think that's backwards."
- Founder bio: Dimitri Dähler — jazz trumpet, HSLU, modular rig, masterproject performance
- Hardware origin story: "This didn't start as a product. It started as a problem."
- Founder photo placeholder (ready for image drop-in)
- Visual design matching brand (navy/teal/magenta)

**Status:** DONE

Plans:
- [x] 02-01: About / manifesto section, founder bio, hardware origin story, manual.html

---

## Phase 03: Legal Pages

**Goal:** Create the three pages required by LemonSqueezy and Stripe before payments go live.

**Delivers:**
- `/privacy.html` — Privacy Policy (GDPR-compliant)
- `/terms.html` — Terms of Service
- `/refund.html` — Refund Policy (no-questions 14-day for plugin, deposit non-refundable for hardware with clear notice)
- Footer links to all three
- Cookie-free analytics note (Plausible = no consent banner needed)

**Status:** DONE

Plans:
- [x] 03-01: Legal pages (privacy.html, terms.html, refund.html)

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

**Goal:** Get the site live on dimea.io with HTTPS, CI/CD preview deploys, and clean URL routing.

**Delivers:**
- Branch renamed master → main (before Vercel connect)
- `.gitignore` committed (OS/editor junk only)
- `vercel.json` committed (5 clean-URL rewrites for /about, /hardware, /impressions, /shop, /app)
- Vercel project connected to dimea-website git repo
- Custom domain `dimea.io` configured with HTTPS
- `www.dimea.io` permanently redirecting to `dimea.io`
- Deploy preview URLs on every git push to non-main branches

**Status:** TODO — 2 plans

Plans:
- [ ] 05-01-PLAN.md — Git cleanup + file creation (branch rename, .gitignore, vercel.json)
- [ ] 05-02-PLAN.md — Vercel connect + domain configuration + live verification

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

**Status:** DONE

Plans:
- [x] 06-01: Warm analog engine (waveforms, filter, envelope, reverb, delay)

---

## Phase 07: Commerce & Link Wiring

**Goal:** Wire all real payment and social URLs into the site so visitors can actually buy the plugin, preorder hardware, and find Dimea Arcade on social media. Prerequisite: LemonSqueezy product + Stripe Payment Link created externally before this phase executes.

**Delivers:**
- "Buy Now" button → real LemonSqueezy checkout URL (plugin, CHF 49 launch price)
- "Try Free" button → demo download link
- "Preorder Hardware" buttons → real Stripe Payment Link (CHF 499, deposit model)
- Canonical URL corrected from `dimea.audio` → `dimea.io` across all HTML files
- Footer social links → real Dimea Arcade accounts (YouTube, Instagram, others)
- Buy Me a Coffee link → real page or removed
- Launch price (CHF 49) shown correctly on site if still in launch period

**Status:** TODO

**External prerequisites (not website tasks):**
- LemonSqueezy account + Arcade Chord Control product listing created
- Stripe Payment Link for hardware deposit created
- Social media accounts created (YouTube, Instagram minimum)

Plans:
- [ ] 07-01: Wire payment links + fix canonical + update social links

---

*Milestone goal: Professional live website ready for plugin launch*
*Created: 2026-03-13*
