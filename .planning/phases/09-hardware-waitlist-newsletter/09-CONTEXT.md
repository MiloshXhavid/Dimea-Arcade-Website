# Phase 09: Hardware Waitlist + Newsletter — Context

**Gathered:** 2026-03-14
**Status:** Ready for planning

<domain>
## Phase Boundary

Connect the two existing placeholder email forms (hardware waitlist in the hardware section + footer newsletter) to Mailchimp. Configure one Mailchimp audience with tags, set up double opt-in, and create welcome email sequences for each signup path. This phase is website + Mailchimp config — the Mailchimp account setup is an external prerequisite done by the user before execution.

</domain>

<decisions>
## Implementation Decisions

### Form Fields
- Both forms: email only (no name field)
- Lower friction at signup — no personalization needed

### Mailchimp Audience Structure
- One Mailchimp audience (free plan — only one audience allowed)
- Mailchimp free plan does NOT support multi-step automations (removed June 2025) — single Final Welcome Email only
- Subscriber differentiation via **Groups** (not tags — tags are not passable via embedded form POST endpoint)
- Hardware waitlist form passes group field for "hardware-waitlist" group
- Footer newsletter form passes group field for "newsletter" group
- Group IDs are audience-specific — Dimitri must create Groups in Mailchimp dashboard and share the `group[CATEGORY_ID][GROUP_ID]` field names before JS can be finalized
- Duplicate emails: Mailchimp deduplicates — second signup adds subscriber to second group

### Form Wiring Method
- Keep existing custom-styled form HTML exactly as-is (no Mailchimp embed HTML replacement)
- Wire via JavaScript fetch/POST to Mailchimp's embedded form POST endpoint (action URL from LS dashboard — no API key needed, works from static sites)
- Hardware waitlist form: `#hwEmailInput` + `handleHwWaitlist()` → replace JS with real Mailchimp POST
- Footer newsletter form: `.footer-email-input` + `.footer-email-btn` → wire with same approach

### Post-Submit Behavior
- Success: show existing inline confirmation text (`.hw-waitlist-confirm` for waitlist, similar inline message for footer)
- Error: show inline error message below the form ("Please enter a valid email" / "Something went wrong — try again")
- No page navigation, no redirect to Mailchimp thank-you page

### Double Opt-In
- Mailchimp double opt-in: ON (required for GDPR/Swiss law compliance)
- Mailchimp sends a confirmation email before subscriber is added to the active list

### Privacy Compliance
- Both forms get a micro-copy privacy note: "By subscribing you agree to our [Privacy Policy]" — link to privacy.html
- Placed below each form's confirmation text area

### Welcome Email Sequences

- **Mailchimp plan: FREE** — single built-in "Final Welcome Email" per audience only (multi-step sequences require Essentials $13/month, not chosen)
- One welcome email covers both signup paths (waitlist + newsletter)
- Claude drafts the welcome email copy based on site content
- Tone: warm and personal — Dimitri writing directly ("Hey, I'm Dimitri...")
- Content: confirm signup, briefly explain DIMEOLA and the plugin, tell them what they'll hear about
- From name: Dimitri / From email: contact@dimea.audio

### From Name & Email (Mailchimp sender config)
- From name: Dimitri
- From email: contact@dimea.audio

### Email Tone
- Warm and personal — Dimitri writing directly to subscribers
- Not corporate, not brand-voice manifesto style
- Honest, direct, founder-to-fan

### External Prerequisites (NOT part of this phase — Dimitri must complete before execution)
- Mailchimp account created (free plan)
- Audience created in Mailchimp
- Two Groups created in the audience: one for "hardware-waitlist", one for "newsletter"
- Mailchimp embedded form HTML generated (Audience → Signup forms → Embedded forms) — provides the action URL and `group[X][Y]` field names
- Double opt-in enabled in Mailchimp audience settings
- Final Welcome Email written and activated in Mailchimp

### Claude's Discretion
- JSONP implementation details (script-tag injection pattern — required since fetch is CORS-blocked by Mailchimp)
- Inline error message styling (reuse existing `.hw-waitlist-confirm` pattern or new element)
- Whether to disable the submit button during POST to prevent double-submit

</decisions>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.hw-waitlist` block (index.html ~line 1386–1401): Complete form UI exists — email input `#hwEmailInput`, "Notify Me" button calling `handleHwWaitlist()`, confirmation div `#hwConfirm`. Just needs JS replaced.
- `.footer-newsletter` block (index.html ~line 1798–1802): Email input `.footer-email-input`, submit button `.footer-email-btn`. Needs JS wired.
- `.hw-waitlist-confirm` / `#hwConfirm`: Already styled confirmation element — reuse this pattern for the footer.
- `.shop-btn--waitlist` (line 1608): Shop section "Join Waitlist →" button scrolls to the hardware waitlist form — this already works, no changes needed.

### Established Patterns
- Flat HTML, vanilla JS inline script blocks — no modules, no imports
- `onclick` attributes and inline event handlers used throughout (see `handleHwWaitlist()`)
- Existing JS (to be replaced) pattern: show/hide `.visible` class on confirm element
- All JS in one `<script>` block near end of `</body>`

### Integration Points
- `handleHwWaitlist()` function (somewhere in inline script): currently fake — replace with real Mailchimp POST
- Footer email button: currently no JS handler — add one
- Mailchimp POST action URL: 1 URL covers both tags (pass different hidden `group` field per form to apply correct tag)

</code_context>

<specifics>
## Specific Details

- Founder name: Dimitri (NOT Dimitar)
- From email: contact@dimea.audio
- Hardware product: DIMEOLA (the hardware device) — distinct from Arcade Chord Control (the plugin)
- Mailchimp Groups (not tags): hardware-waitlist group + newsletter group — IDs provided by Dimitri from dashboard
- JSONP callback URL format: change `/post?` to `/post-json?` and append `&c=CALLBACK_NAME`
- Confirmation text update needed: current `#hwConfirm` says "You're on the list. We'll be in touch." — with double opt-in ON this must change to "Check your inbox — click the link to confirm your spot."
- Privacy policy URL: `/privacy` (routes to privacy.html via vercel.json rewrite)
- Execution prerequisite: Dimitri sets up Mailchimp account + groups + gets action URL BEFORE executing this plan

</specifics>

<deferred>
## Deferred Ideas

- Mailchimp segmentation for future broadcast targeting (future when list grows)
- A/B testing subject lines (future)
- Unsubscribe page customization (Mailchimp default is fine for now)
- SMS waitlist option (not in scope)

</deferred>

---

*Phase: 09-hardware-waitlist-newsletter*
*Context gathered: 2026-03-14*
