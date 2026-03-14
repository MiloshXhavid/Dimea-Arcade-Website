# Phase 09: Hardware Waitlist + Newsletter — Research

**Researched:** 2026-03-14
**Domain:** Mailchimp embedded form integration, vanilla JS JSONP, email automation
**Confidence:** HIGH (technical pattern) / HIGH (Mailchimp free plan limitations — verified, critical)

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

- Both forms: email only (no name field)
- One Mailchimp audience (free plan allows only one)
- Hardware waitlist subscribers tagged: `hardware-waitlist`
- Footer newsletter subscribers tagged: `newsletter`
- Keep existing custom-styled form HTML exactly as-is (no Mailchimp embed HTML replacement)
- Wire via JavaScript fetch/POST to Mailchimp's embedded form POST endpoint (action URL from LS dashboard — no API key needed, works from static sites)
- Hardware waitlist form: `#hwEmailInput` + `handleHwWaitlist()` → replace JS with real Mailchimp POST
- Footer newsletter form: `.footer-email-input` + `.footer-email-btn` → wire with same approach
- Success: show existing inline confirmation text (`.hw-waitlist-confirm` for waitlist, similar inline message for footer)
- Error: show inline error message below the form ("Please enter a valid email" / "Something went wrong — try again")
- No page navigation, no redirect to Mailchimp thank-you page
- Mailchimp double opt-in: ON (required for GDPR/Swiss law compliance)
- Both forms get micro-copy privacy note: "By subscribing you agree to our [Privacy Policy]" — link to privacy.html
- Two-email welcome sequence for hardware-waitlist (Email 1 immediate, Email 2 +1 day after Email 1)
- Single welcome email for newsletter tag
- From name: Dimitri. From email: contact@dimea.audio
- Flat HTML site, vanilla JS, Vercel static hosting — no backend, no modules

### Claude's Discretion

- Exact JS implementation for the fetch → Mailchimp POST (JSONP vs hidden iframe vs fetch with no-cors)
- Inline error message styling (reuse existing `.hw-waitlist-confirm` pattern or new element)
- Whether to disable the submit button during POST to prevent double-submit

### Deferred Ideas (OUT OF SCOPE)

- Mailchimp segmentation for future broadcast targeting
- A/B testing subject lines
- Unsubscribe page customization
- SMS waitlist option
</user_constraints>

---

## Summary

This phase wires two existing placeholder email forms (hardware waitlist + footer newsletter) to Mailchimp using their public embedded form POST endpoint. The integration requires no API key and works from Vercel static hosting.

The critical discovery from research: **Mailchimp's free plan no longer supports automated email sequences as of June 2025.** Classic Automations were retired. The Customer Journey Builder (replacement) requires a paid Essentials plan ($13/month). The only email automated on the free plan is Mailchimp's built-in "Final Welcome Email" — one email, triggered post-confirmation. The two-email hardware waitlist sequence locked in CONTEXT.md requires a paid plan.

The correct technical approach for the JavaScript integration is JSONP (script tag injection), not `fetch()`. Mailchimp's embedded form endpoint does not send CORS headers, so `fetch()` will silently fail. JSONP bypasses CORS entirely by injecting a `<script>` tag.

**Primary recommendation:** Use JSONP script-tag injection for both forms. For the waitlist form, pass a Mailchimp "Group" hidden field (not a Tag — Tags cannot be passed via embedded form POST) to differentiate subscribers at signup. Plan must include a note that the email sequences require Mailchimp Essentials ($13/month) — confirm with Dimitri before activating sequences.

---

## Standard Stack

### Core
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Mailchimp embedded form endpoint | n/a | Public subscribe POST, no API key | The only CORS-safe client-side Mailchimp method |
| Vanilla JS JSONP (script tag) | n/a | Submit to Mailchimp from static site | fetch() blocked by CORS, JSONP is established workaround |

### Supporting
| Component | Version | Purpose | When to Use |
|-----------|---------|---------|-------------|
| Mailchimp Groups | n/a | Differentiating subscribers at signup | Use instead of Tags — Tags cannot be passed via embedded form POST |
| Mailchimp Final Welcome Email | n/a | Single auto-sent email post-confirmation | Only free-plan automation option |
| Mailchimp Essentials plan | $13/mo | Multi-step Customer Journey sequences | Required for 2-email hardware waitlist sequence |

### Alternatives Considered
| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Mailchimp | MailerLite, EmailOctopus | Better free plans (500+ contacts, automation included), but CONTEXT.md locks Mailchimp |
| JSONP | fetch() with no-cors mode | no-cors hides response — cannot detect success/error |
| JSONP | Hidden iframe POST | Works but no response callback — cannot detect success/error |

**Installation:** No npm packages. Pure vanilla JS — JSONP pattern implemented inline.

---

## Architecture Patterns

### Recommended Project Structure

No new files needed. All changes are within:
```
index.html
  ├── JS inline script block (~line 1863): replace handleHwWaitlist() + add footer handler
  └── HTML (~lines 1386-1401, 1798-1802): add privacy note below each form
```

### Pattern 1: Mailchimp JSONP Submit (Script Tag Injection)

**What:** Converts the Mailchimp embedded form action URL from `/post?` to `/post-json?`, appends form data as query string, injects a `<script>` tag pointing to the URL, Mailchimp responds by calling a globally defined callback function.

**When to use:** Any time you POST to Mailchimp from a static site (no backend). Required because Mailchimp's endpoint does not include CORS headers.

**Example:**
```javascript
// Source: CSS-Tricks "Form Validation Part 4" + Mailchimp embedded form docs
function mailchimpSubscribe(email, callbackName, actionUrl) {
  // Build JSONP URL: swap /post? → /post-json?, append email + callback
  var url = actionUrl.replace('/post?', '/post-json?');
  url += '&EMAIL=' + encodeURIComponent(email) + '&c=' + callbackName;

  // Define global callback Mailchimp will invoke
  window[callbackName] = function(data) {
    delete window[callbackName];
    if (data.result === 'success') {
      // show success UI
    } else {
      // show error UI — data.msg contains reason
    }
  };

  // Inject script tag — Mailchimp calls window[callbackName](responseObj)
  var script = document.createElement('script');
  script.src = url;
  document.head.appendChild(script);
  script.onload = function() { this.remove(); };
}
```

### Pattern 2: Mailchimp Group Hidden Field for Subscriber Differentiation

**What:** Tags cannot be submitted via embedded form POST. Mailchimp Groups can. Add a hidden checkbox input pre-checked with `checked` to the JSONP URL params to automatically assign subscribers to a group at signup. Groups serve the same internal-segmentation purpose as Tags in this use case.

**Critical note:** The group ID and group category ID are obtained from the Mailchimp audience's embedded form HTML. Dimitri must provide these IDs from his Mailchimp dashboard after creating two groups ("Hardware Waitlist" and "Newsletter") in the audience.

**Field name format (from Mailchimp docs):**
```
group[GROUP_CATEGORY_ID][GROUP_ID]=1
```

**JSONP URL with group field:**
```javascript
var url = actionUrl.replace('/post?', '/post-json?');
url += '&EMAIL=' + encodeURIComponent(email);
url += '&group[01245][8]=1';   // IDs from Mailchimp dashboard — replace before launch
url += '&c=mailchimpCallback';
```

### Pattern 3: CONTEXT.md Tags → Mailchimp Groups Mapping

Because Tags cannot be passed via embedded form POST, the implementation uses Mailchimp **Groups** (which CAN be passed) rather than Tags. The groups are hidden at signup. This achieves identical segmentation capability:

| CONTEXT.md locked decision | Implementation reality |
|---------------------------|----------------------|
| Tag: `hardware-waitlist` | Mailchimp Group: "Hardware Waitlist" (hidden, auto-selected) |
| Tag: `newsletter` | Mailchimp Group: "Newsletter" (hidden, auto-selected) |

This is not a violation of the locked decision — it achieves the same goal (differentiated subscriber lists within one audience) using the mechanism Mailchimp actually supports for embedded forms.

### Pattern 4: Two-Function JS Architecture

Both forms get separate handler functions. Share the common JSONP logic. The existing `handleHwWaitlist()` function is replaced in-place; a new `handleFooterNewsletter()` function is added.

```javascript
// Shared JSONP submitter
function submitToMailchimp(email, groupParam, onSuccess, onError) { ... }

// Waitlist handler (replaces existing handleHwWaitlist)
function handleHwWaitlist() {
  var email = document.getElementById('hwEmailInput').value;
  // validate, then:
  submitToMailchimp(email, '&group[CATEGORY_ID][HW_GROUP_ID]=1', onSuccess, onError);
}

// Footer handler (new)
function handleFooterNewsletter() {
  var email = document.querySelector('.footer-email-input').value;
  submitToMailchimp(email, '&group[CATEGORY_ID][NL_GROUP_ID]=1', onSuccess, onError);
}
```

### Anti-Patterns to Avoid

- **fetch() to Mailchimp:** Will fail silently on CORS without `no-cors`, and `no-cors` hides the response — cannot detect success or error. Do not use.
- **fetch() with no-cors mode:** Response type is "opaque" — `data.result` is inaccessible. Cannot show correct success/error UI.
- **Using Mailchimp Tags instead of Groups:** Tags cannot be submitted via embedded form POST endpoint. Mailchimp Groups are the correct mechanism for automatic subscriber segmentation at signup.
- **One global callback name for both forms:** If both forms submit simultaneously, the second callback will overwrite the first. Use unique callback names per form (e.g., `mcCallbackHw`, `mcCallbackFooter`).
- **Showing success before JSONP callback fires:** The script tag insertion is async — do not show success UI until `data.result === 'success'` in the callback.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Email validation | Custom regex | HTML `type="email"` + `includes('@')` check | The existing pattern already does this; browser handles format |
| Subscriber deduplication | Custom duplicate check | Mailchimp handles it server-side | Mailchimp merges duplicate emails, adding new group to existing record |
| Confirmation email | Custom email sending | Mailchimp double opt-in system | Built in, handles deliverability, GDPR-compliant |
| Welcome email sequences | Custom email sending | Mailchimp automations (paid plan) | Deliverability, unsubscribe handling, CAN-SPAM compliance |

**Key insight:** The JSONP integration is deliberately thin — submit email + group field, handle two outcomes (success/error). Everything else (dedup, confirmation, welcome emails) is Mailchimp's responsibility.

---

## Common Pitfalls

### Pitfall 1: fetch() CORS Failure (Silent)
**What goes wrong:** Developer uses `fetch(mailchimpUrl, { mode: 'no-cors' })` — request sends but response is opaque. `data.result` is always undefined. No visible error, but success/error UI never works correctly.
**Why it happens:** Mailchimp's endpoint does not include `Access-Control-Allow-Origin` headers for cross-origin requests.
**How to avoid:** Use JSONP script tag injection exclusively — it predates CORS and bypasses it entirely.
**Warning signs:** Network tab shows 200 OK but the callback never fires; or response body is empty.

### Pitfall 2: Mailchimp "Already Subscribed" Error Message
**What goes wrong:** `data.result === 'error'` with `data.msg` containing "is already subscribed to list." The form shows an error for someone who's a valid returning subscriber.
**Why it happens:** Mailchimp returns result='error' for already-subscribed emails.
**How to avoid:** Parse `data.msg` — if it contains "already subscribed", show the success confirmation instead of error (they're already on the list). Or show a neutral "Check your inbox — you may already be subscribed."
**Warning signs:** Users report form showing error even though they signed up successfully previously.

### Pitfall 3: Automation Plan Mismatch
**What goes wrong:** Welcome email sequences are configured in Mailchimp Customer Journey builder but never fire because the account is on the free plan.
**Why it happens:** As of June 2025, Mailchimp removed all automation from the free plan. Customer Journey builder shows sequence design but cannot activate without Essentials ($13/month).
**How to avoid:** Confirm with Dimitri whether to upgrade to Essentials before activating email sequences. The sequences can be authored as part of this phase, but activation requires paid plan.
**Warning signs:** Sequence shows as "inactive" or "requires upgrade" in Mailchimp dashboard.

### Pitfall 4: Group IDs Missing / Not Obtained from Dashboard
**What goes wrong:** Code has placeholder group IDs that are never replaced with real ones. Subscribers arrive in Mailchimp with no group assigned — cannot differentiate hardware vs newsletter subscribers.
**Why it happens:** Group IDs are not discoverable without accessing Mailchimp dashboard and looking at the embedded form HTML. They are audience-specific and cannot be known in advance.
**How to avoid:** The plan must clearly note this as a prerequisite: Dimitri creates two Groups in his audience, then provides the exact embedded form HTML snippet so the real group IDs can be extracted and placed in the JS.
**Warning signs:** All subscribers appear in Mailchimp with no group/tag; cannot distinguish hardware vs newsletter signups.

### Pitfall 5: Mailchimp Free Plan Contact Limit (250)
**What goes wrong:** Account hits 250 contact limit; new subscribers are rejected by Mailchimp (returns error response).
**Why it happens:** Mailchimp free plan is capped at 250 total contacts including unsubscribed.
**How to avoid:** Note this limit in plan. Once list approaches 250, upgrade to Essentials ($13/month). For an early-stage waitlist this is unlikely to be immediately blocking, but should be documented.

### Pitfall 6: Double Opt-In vs. Final Welcome Email Timing
**What goes wrong:** User submits form → form shows "You're on the list" → but they haven't confirmed yet (double opt-in). They may not realize they need to check their email to confirm.
**Why it happens:** The confirmation step happens asynchronously after form submission.
**How to avoid:** Change the success confirmation text to reflect the double opt-in flow: "Check your inbox — click the link to confirm your spot." Do not say "You're on the list" until they've confirmed.
**Warning signs:** High signup rate but low confirmed subscriber rate.

---

## Code Examples

Verified patterns from official sources and CSS-Tricks documentation:

### Complete JSONP Submit Function (Vanilla JS)
```javascript
// Source: CSS-Tricks Form Validation Part 4 (css-tricks.com/form-validation-part-4-validating-mailchimp-subscribe-form/)
// Adapted for this project's inline-script pattern

var MC_ACTION_URL = 'https://YOURSERVER.us1.list-manage.com/subscribe/post?u=USERID&id=LISTID';
// ^ Replace with real URL from Mailchimp > Audience > Signup forms > Embedded forms

function submitToMailchimp(email, groupParam, onSuccess, onError) {
  var callbackName = 'mcCallback_' + Date.now();
  var url = MC_ACTION_URL.replace('/post?', '/post-json?');
  url += '&EMAIL=' + encodeURIComponent(email);
  url += groupParam; // e.g. '&group[01245][8]=1'
  url += '&c=' + callbackName;

  window[callbackName] = function(data) {
    delete window[callbackName];
    if (data.result === 'success') {
      onSuccess();
    } else {
      // "already subscribed" is not a real error — treat as success
      if (data.msg && data.msg.indexOf('already subscribed') !== -1) {
        onSuccess();
      } else {
        onError(data.msg || 'Something went wrong — try again');
      }
    }
  };

  var script = document.createElement('script');
  script.src = url;
  document.head.appendChild(script);
  script.onload = function() { this.remove(); };
}
```

### Updated handleHwWaitlist() (replaces existing stub)
```javascript
// Replaces the existing handleHwWaitlist() in the inline <script> block
var HW_GROUP_PARAM = '&group[CATEGORY_ID][HW_GROUP_ID]=1'; // Fill from Mailchimp dashboard

function handleHwWaitlist() {
  var input = document.getElementById('hwEmailInput');
  var btn = document.querySelector('.hw-email-btn');
  var confirm = document.getElementById('hwConfirm');
  var errorEl = document.getElementById('hwError'); // New element to add

  if (!input.value || !input.value.includes('@')) {
    input.style.borderColor = 'rgba(232,58,110,0.6)';
    setTimeout(function() { input.style.borderColor = ''; }, 800);
    return;
  }

  input.disabled = true;
  btn.disabled = true;

  submitToMailchimp(input.value, HW_GROUP_PARAM,
    function() { // success
      confirm.classList.add('visible');
      btn.textContent = 'Done ✓';
    },
    function(msg) { // error
      input.disabled = false;
      btn.disabled = false;
      if (errorEl) { errorEl.textContent = msg; errorEl.style.display = 'block'; }
    }
  );
}
```

### Mailchimp Endpoint URL Format
```
Standard:   https://YOURSERVER.us1.list-manage.com/subscribe/post?u=USERID&id=LISTID
JSONP:      https://YOURSERVER.us1.list-manage.com/subscribe/post-json?u=USERID&id=LISTID&EMAIL=...&group[X][Y]=1&c=callbackName
```
The `YOURSERVER`, `USERID`, and `LISTID` values come from Mailchimp > Audience > Signup forms > Embedded forms > Copy form code.

### Group Hidden Field in Mailchimp Dashboard HTML
```html
<!-- This is what Mailchimp generates in the embedded form code — extract the IDs from here -->
<input type="checkbox" value="8" name="group[01245][8]"
  id="mce-group[01245]-01245-0" checked>
<!-- group[CATEGORY_ID][GROUP_ID] — use these numbers in your JS -->
```

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| jQuery $.ajax JSONP | Vanilla JS script tag injection | 2020+ (jQuery dropped) | No jQuery dependency; same mechanism |
| Classic Automations | Customer Journey Builder | June 2025 | Multi-step sequences require Essentials ($13/mo) |
| Free plan: 2,000 contacts | Free plan: 250 contacts | Early 2026 | Severely limits list size before upgrade needed |
| Tags passable via form | Groups only via embedded form | Always | Tags are internal-only; Groups are the form-native equivalent |

**Deprecated/outdated:**
- **Classic Automation Builder:** Retired June 2025. All existing Classic Automations migrated or stopped. Do not reference "Classic Automations" in setup docs — use Customer Journey Builder (paid).
- **$.ajax jsonp:true (jQuery):** Still functional but jQuery not in this project. Use script tag injection instead.
- **fetch() with no-cors:** Technically works for sending, useless for response handling. Not a valid approach.

---

## Open Questions

1. **Mailchimp plan: free vs paid for sequences**
   - What we know: Free plan (250 contacts, no automation); Essentials ($13/month, 500 contacts, Customer Journey Builder, multi-step sequences)
   - What's unclear: Does Dimitri want to pay $13/month for the email sequences? Or accept a single welcome email (Final Welcome Email, free)?
   - Recommendation: Plan should author all email copy regardless. Add a prerequisite note: "Confirm with Dimitri whether to upgrade to Essentials before activating sequences." Provide both paths: (A) free plan path with single Final Welcome Email, (B) paid path with 2-email sequence.

2. **Mailchimp Group IDs (audience-specific)**
   - What we know: Group IDs are set by Mailchimp when Dimitri creates groups in his audience. Not knowable until then.
   - What's unclear: The exact `group[CATEGORY_ID][GROUP_ID]` values.
   - Recommendation: Plan must state: "Dimitri creates two Groups in Mailchimp audience ('Hardware Waitlist', 'Newsletter'), then shares the embedded form HTML snippet so group IDs can be extracted."

3. **CONTEXT.md says "tags" but embedded forms only support groups**
   - What we know: Tags are internal-only labels in Mailchimp. The embedded form POST endpoint only supports Groups for subscriber-segment differentiation. Groups achieve the same purpose.
   - What's unclear: Whether Dimitri has a strong preference for "Tags" specifically (e.g., for future Campaign filtering by tag).
   - Recommendation: Implement using Groups. Document that Groups serve the same segmentation function. Tags can be applied retroactively via Mailchimp automation rule "contact joins group → apply tag" on a paid plan.

---

## Validation Architecture

> `nyquist_validation` key absent from config.json — treated as enabled.

### Test Framework
| Property | Value |
|----------|-------|
| Framework | None — flat HTML/vanilla JS project, no test framework configured |
| Config file | None |
| Quick run command | Manual browser test: open index.html via Vercel preview URL |
| Full suite command | N/A |

### Phase Requirements → Test Map
| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| 09-A | Waitlist form submits to Mailchimp JSONP endpoint | manual | Load preview URL, submit real email, verify Mailchimp audience receives it | N/A |
| 09-B | Footer newsletter form submits to Mailchimp | manual | Load preview URL, submit real email, check Mailchimp | N/A |
| 09-C | Success UI shows after confirmed submit | manual | Check `#hwConfirm.visible` appears; check footer confirm appears | N/A |
| 09-D | Error UI shows for invalid email | manual | Submit non-email, check red border + error message | N/A |
| 09-E | Privacy note visible below both forms | manual | Visual inspection | N/A |
| 09-F | Double opt-in confirmation email arrives | manual | Submit real email, check inbox for Mailchimp confirmation | N/A |
| 09-G | Subscribers tagged/grouped correctly in Mailchimp | manual | Log in to Mailchimp, check audience for correct group assignment | N/A |

### Wave 0 Gaps
- No test files exist — this project has no automated test infrastructure
- All validation is manual (browser + Mailchimp dashboard inspection)
- No gaps to fill at Wave 0 for test files; manual steps cover all requirements

*(This is a flat HTML/vanilla JS project with no test framework. All phase validation is manual. This is consistent with the project's established testing approach across all prior phases.)*

---

## Sources

### Primary (HIGH confidence)
- [CSS-Tricks: Form Validation Part 4 — Mailchimp](https://css-tricks.com/form-validation-part-4-validating-mailchimp-subscribe-form/) — JSONP script tag injection pattern, full vanilla JS implementation
- [Mailchimp: Automatically Add Subscribers to a Group at Signup](https://mailchimp.com/help/automatically-add-subscribers-to-a-group-at-signup/) — `group[CATEGORY_ID][GROUP_ID]` hidden field format
- [Mailchimp: About Double Opt-in](https://mailchimp.com/help/about-double-opt-in/) — confirmation email flow, final welcome email
- [Mailchimp: Pricing Page](https://mailchimp.com/pricing/) — free plan: 250 contacts, 500 emails/month, 1 audience
- [Mailchimp: About Customer Journeys](https://mailchimp.com/help/about-customer-journeys/) — multi-step flows require paid plan

### Secondary (MEDIUM confidence)
- [Sender.net: Mailchimp Free Plan 2026](https://www.sender.net/reviews/mailchimp/pricing/free-plan/) — confirms free plan automation removed, 250 contact limit
- [BrieBug: Mailchimp JSONP with Angular](https://blog.briebug.com/blog/mailchimp-subscribe-form-with-angular-using-jsonp) — JSONP mechanism and `c=` callback parameter
- [jfelix.info: Kick-Start Newsletter with Mailchimp](https://jfelix.info/blog/kick-start-your-newsletter-mailchimp-custom-form-with-react) — endpoint URL transformation, response object format (`result`, `msg`)

### Tertiary (LOW confidence — flag for validation)
- [blog.groupmail.io: Mailchimp Free Plan Changes 2026](https://blog.groupmail.io/mailchimp-free-plan-changes-2026/) — confirms automation removal June 2025 (third-party blog; verify against official Mailchimp docs before assuming)

---

## Metadata

**Confidence breakdown:**
- JSONP integration pattern: HIGH — documented in multiple official/authoritative sources, well-established technique
- Mailchimp free plan automation removal: HIGH — confirmed by official pricing page + multiple independent sources
- Group field name format: HIGH — from official Mailchimp help documentation
- Email sequence setup (Customer Journeys): MEDIUM — flow is documented, but exact trigger configuration for "group joined" depends on Mailchimp dashboard UI at time of setup
- Group IDs: LOW — cannot be known until Dimitri creates audience and provides form HTML

**Research date:** 2026-03-14
**Valid until:** 2026-06-14 (Mailchimp pricing/features stable over 90-day window; verify free plan limits before execution if delayed)
