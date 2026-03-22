# Phase 10: Site Reframe — Plugin First

**Gathered:** 2026-03-15
**Status:** Ready for planning
**Source:** Conversation context (user decisions)

<domain>
## Phase Boundary

Reframe the website from a dual plugin+hardware sales site to a plugin-first site. The Arcade Chord Control plugin is the product being sold today. Hardware (DIMEOLA) and App are future vision sections — they build brand and show roadmap, but they are not products for sale. No deposit, no preorder, no pricing on unbuilt products.

This phase replaces the original Phase 10 scope (Stripe hardware deposit), which was deferred because the hardware is still at concept/prototype stage, pricing is not finalized, and spec can change significantly before Kickstarter.

</domain>

<decisions>
## Implementation Decisions

### Hero Section
- Remove "Preorder Hardware — CHF 499" primary button entirely
- "Get Plugin — CHF 49" becomes the primary CTA (btn-primary)
- "Explore Free →" stays as the secondary / ghost CTA
- Hero subtitle should be audited — remove or soften any hardware sales language
- Hero communicates: plugin is available now

### Hardware Section (pageHardware)
- Keep the section — it tells the brand story and shows vision
- Remove all pricing (CHF 499, "~CHF 499") from status bar and any other elements
- Remove any deposit / preorder CTAs or language
- Waitlist form stays (Phase 09 wires it to Mailchimp — that work is separate)
- Reframe section tone: "this is where we're headed" not "buy this"
- The hardware section label/status can say something like "In Development" or "Coming"

### App Section (pageApp)
- Same treatment as hardware — vision framing, no price, no purchase CTA
- "Coming Soon" or "In Development" badge/label
- No CTA beyond awareness / waitlist if applicable

### Shop Section (pageShop)
- Collapse from 3-card grid to plugin-only
- Remove DIMEOLA preorder card
- Remove Bundle card
- Add a small "More coming" note below the plugin card (not another card — just a line or small text element)
- Shop is now a plugin sales section, not a multi-product catalog

### Navigation
- "Hardware" nav link → rename label to "Vision" (keeps the anchor, points to hardware section)
- "App" nav link → rename label to "Vision" or fold into the same concept
  - If both Hardware and App sections exist and are both "vision" sections, consider whether two separate nav links both called "Vision" is confusing — Claude's discretion on naming (e.g. "Roadmap", "Future", or keep distinct names)
- Section divider labels should be updated to match

### Claude's Discretion
- Exact wording for "In Development" badges / labels
- Whether to add a one-liner in the hardware section header like "Not for sale yet — follow the build"
- How to handle the mobile nav (same label renames apply)
- Footer navigation link labels

</decisions>

<specifics>
## Specific Details

- Site: flat HTML (index.html) — no framework, vanilla JS
- Hosted on Vercel (static)
- Phase 09 (Mailchimp waitlist) handles the hw-waitlist form — do NOT remove or rewire that form in this phase
- Plugin price: CHF 49 launch / CHF 79 regular — stays as-is
- Hardware price: REMOVE from all visible elements
- App: no price was shown, just mark as vision/coming

</specifics>

<deferred>
## Deferred / Out of Scope

- Stripe hardware deposit → deferred until Kickstarter-ready (working prototype on video, confirmed specs + pricing, manufacturing quote)
- Kickstarter campaign → Phase 19 (v5.0 milestone)
- Phase 10 original scope (Stripe Payment Link wiring) is cancelled for now
- Crypto checkout for hardware → Phase 11 (still applies to plugin, hardware deposit deferred)

</deferred>

---

*Phase: 10-site-reframe-plugin-first*
*Context gathered: 2026-03-15 via conversation*
