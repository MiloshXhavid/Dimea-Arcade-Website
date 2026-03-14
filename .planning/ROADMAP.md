# Dimea Arcade Website — Master Roadmap

---

## Milestone Overview

| Milestone | Goal | Status |
|-----------|------|--------|
| **v1.0 — Launch-Ready Site** | Live site, professional quality, plugin sales + hardware waitlist | ⏳ In Progress |
| v2.0 — Full Commerce | Plugin purchasable, hardware deposit live, crypto payments | 🔒 Blocked (Mac signing ~2026-03-21) |
| v3.0 — Trust & Support | Testimonials, DAW compatibility, FAQ, analytics | ⏳ Queued |
| v4.0 — Growth & Content | Blog, press kit, social, app landing page | ⏳ Queued |
| v5.0 — Kickstarter Campaign | DIMEOLA campaign page, reward tiers, backer comms | ⏳ Queued |

---

<!-- ▶ CURRENT MILESTONE: v1.0 — Launch-Ready Site -->

## Phase 01: Fix, Polish & Mobile

**Goal:** Take the existing index.html from prototype to production-ready.

**Status:** DONE

Plans:
- [x] 01-01: Fix, polish & mobile (audio engine, responsive layout, embeds)

---

## Phase 02: About / Story / Vision

**Goal:** Add the founder story, manifesto, and brand vision as a dedicated section.

**Status:** DONE

Plans:
- [x] 02-01: About / manifesto section, founder bio, hardware origin story, manual.html

---

## Phase 03: Legal Pages

**Goal:** Create the three pages required by LemonSqueezy and Stripe before payments go live.

**Status:** DONE

Plans:
- [x] 03-01: Legal pages (privacy.html, terms.html, refund.html)

---

## Phase 04: SEO, Meta & Favicon

**Goal:** Make the site look good when shared, rank for relevant searches, and have a proper favicon.

**Status:** DONE

Plans:
- [x] 04-01: Meta tags + OG tags + sitemap + robots.txt
- [x] 04-02: Favicon files + OG image

---

## Phase 05: Domain + Vercel Deploy

**Goal:** Get the site live on dimea.io with HTTPS, CI/CD preview deploys, and clean URL routing.

**Status:** In Progress — 1/2 plans complete

Plans:
- [x] 05-01: Git cleanup + Vercel connect — live at dimea-arcade-website-cfy8wg6sg-miloshxhavids-projects.vercel.app
- [ ] 05-02: Domain purchase + DNS — deferred ~2026-03-21 (waiting for Mac signing + commerce setup)

---

## Phase 06: Joystick Audio — Warm Analog Sound

**Goal:** Transform the joystick XY pad into a warm, lush, analog-feeling synth pad.

**Status:** DONE

Plans:
- [x] 06-01: Warm analog engine (waveforms, filter, envelope, reverb, delay)

---

## Phase 07: Commerce & Link Wiring

**Goal:** Wire all real payment and social URLs into the site.

**Status:** TODO — BLOCKED (external prereqs)

**External prerequisites:**
- Mac plugin signing complete
- LemonSqueezy account + Arcade Chord Control product listing created
- Stripe Payment Link for hardware deposit created
- Social media accounts created (YouTube, Instagram minimum)

Plans:
- [ ] 07-01: Wire payment links + fix canonical + update social links

---

<!-- MILESTONE: v2.0 — Full Commerce (starts ~2026-03-21) -->

## Phase 08: LemonSqueezy Plugin Checkout

**Goal:** Plugin purchasable directly from the site via overlay checkout.

**Delivers:**
- LS overlay JS snippet added to index.html
- "Buy Now" button triggers LS overlay (not redirect)
- Launch price CHF 49 / regular CHF 79 configured in LS
- Post-purchase: customer receives download link + license key by email (LS handles automatically)

**Status:** TODO

**External prerequisites:**
- LemonSqueezy account + Arcade Chord Control product listing created
- Windows installer uploaded to LemonSqueezy

Plans:
- [ ] 08-01: Wire LemonSqueezy overlay checkout into index.html

---

## Phase 09: Hardware Waitlist + Newsletter

**Goal:** Collect interested buyers for DIMEOLA before Kickstarter. Build the email list now.

**Delivers:**
- Mailchimp audience configured (launch list)
- Waitlist form wired to Mailchimp JSONP in hardware section
- General newsletter signup in footer wired to Mailchimp JSONP
- Double opt-in confirmation email (Mailchimp handles)
- Single Final Welcome Email (free plan) — drafted and activated in Mailchimp

**Status:** TODO — 2 plans

Plans:
- [ ] 09-01-PLAN.md — Mailchimp setup checkpoint + draft welcome email copy
- [ ] 09-02-PLAN.md — Wire JSONP integration into index.html (both forms)

---

## Phase 10: Stripe Hardware Deposit / Preorder

**Goal:** Accept deposits for DIMEOLA hardware. Validates demand + generates early revenue.

**Delivers:**
- "Preorder Hardware" button → Stripe Payment Link
- Clear messaging: "deposit secures your unit — remainder due on ship"
- Refund policy link visible before checkout

**Status:** TODO

**External prerequisites:**
- Stripe Payment Link for hardware deposit (CHF 99–149) created

Plans:
- [ ] 10-01: Wire Stripe deposit Payment Link into hardware section

---

## Phase 11: Crypto Checkout

**Goal:** Accept BTC, ETH, USDC, SOL for plugin purchases and hardware deposits.

**Delivers:**
- Coinbase Commerce account set up
- Plugin section: crypto checkout button alongside LemonSqueezy overlay
- Hardware section: crypto deposit option alongside Stripe
- Clear buyer instructions

**Status:** TODO

**External prerequisites:**
- Coinbase Commerce account + charge links created

Plans:
- [ ] 11-01: Add crypto checkout options to plugin + hardware sections

---

<!-- MILESTONE: v3.0 — Trust & Support -->

## Phase 12: Testimonials + Social Proof

**Status:** TODO

---

## Phase 13: DAW Compatibility + System Requirements + FAQ

**Status:** TODO

---

## Phase 14: Analytics (Plausible)

**Status:** TODO

---

<!-- MILESTONE: v4.0 — Growth & Content -->

## Phase 15: Blog / Changelog

**Status:** TODO

---

## Phase 16: Press Kit

**Status:** TODO

---

## Phase 17: Social + Community

**Status:** TODO

---

## Phase 18: App Landing Page

**Status:** TODO

---

<!-- MILESTONE: v5.0 — Kickstarter Campaign -->

## Phase 19: Kickstarter Campaign Page

**Status:** TODO

---

## Phase 20: Backer Communication

**Status:** TODO

---

## Phase 21: Campaign Launch

**Status:** TODO

---

*Created: 2026-03-13 | Last updated: 2026-03-14*
