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
- One Mailchimp audience (free plan allows only one)
- Hardware waitlist subscribers tagged: `hardware-waitlist`
- Footer newsletter subscribers tagged: `newsletter`
- Duplicate emails: Mailchimp deduplicates — second signup adds the second tag to the existing record (no duplicate contacts)

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

#### Hardware Waitlist (tag: hardware-waitlist)
Two-email sequence, triggered on confirmed subscription:

**Email 1 — Immediate (confirmation + what DIMEOLA is):**
- Claude drafts copy based on site content (manifesto, about section, hardware section)
- Tone: warm and personal — Dimitri writing directly to the subscriber ("Hey, I'm Dimitri...")
- Content: confirm they're on the list + 2-3 sentence explanation of what DIMEOLA is and why it exists

**Email 2 — 1 day after Email 1 (timeline + what to expect):**
- Content: Kickstarter timeline with `[TIMELINE PLACEHOLDER]` for Dimitri to fill in before activating
- What milestones remain, what early backers get, what to expect next
- Same warm/personal tone

#### Footer Newsletter (tag: newsletter)
One welcome email:
- Tone: warm and personal
- Content: thanks for subscribing + brief "here's what you'll get" (hardware updates, plugin updates, tutorials)
- No sequence — single email, then they receive future broadcast campaigns

### From Name & Email (Mailchimp sender config)
- From name: Dimitri
- From email: contact@dimea.audio

### Email Tone
- Warm and personal — Dimitri writing directly to subscribers
- Not corporate, not brand-voice manifesto style
- Honest, direct, founder-to-fan

### External Prerequisites (NOT part of this phase)
- Mailchimp account created
- Audience created in Mailchimp
- Mailchimp embedded form POST action URL obtained from audience settings
- Mailchimp automation/Customer Journeys enabled for welcome sequences

### Claude's Discretion
- Exact JS implementation for the fetch → Mailchimp POST (JSONP vs hidden iframe vs fetch with no-cors)
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
- Mailchimp tag for hardware form: `hardware-waitlist`
- Mailchimp tag for footer form: `newsletter`
- Timeline in Email 2: leave as `[TIMELINE PLACEHOLDER]` — Dimitri fills in before activating the sequence
- Privacy policy URL: `/privacy` (routes to privacy.html via vercel.json rewrite)

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
