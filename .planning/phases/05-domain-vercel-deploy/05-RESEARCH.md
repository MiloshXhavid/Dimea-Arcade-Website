# Phase 05: Domain + Vercel Deploy — Research

**Researched:** 2026-03-14
**Domain:** Vercel deployment, custom DNS, static site config, git workflow
**Confidence:** HIGH (all findings verified against official Vercel docs)

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Domain**
- Chosen domain: `dimea.io`
- Registration: 3-year term (~CHF 150)
- Registrar: TBD — DNS steps must be generic (registrar-agnostic)
- Canonical: apex (`dimea.io`) is canonical; `www.dimea.io` redirects to apex (Vercel handles automatically when both are added)

**Repository**
- Branch rename: `master` → `main` before Vercel connect
- Production deploys on push to `main`; preview URLs on all other branches
- Solo dev — no branch protection rules needed
- `.gitignore`: OS/editor junk only (`.DS_Store`, `Thumbs.db`, `desktop.ini`). `.planning/` and `.claude/` stay in git intentionally.

**Vercel Config**
- Project root: repo root (flat HTML, no build step, no framework preset — set to "Other")
- Output directory: `.` (repo root)
- Custom 404: plain Vercel default — no custom 404 page
- `manual.html`: unlisted only (already `noindex` in meta) — no Vercel-level restriction needed

**URL Rewrites**
- `/about` → `/index.html`
- `/hardware` → `/index.html`
- `/impressions` → `/index.html`
- `/shop` → `/index.html`
- `/app` → `/index.html`
- Anchors (`#pageAbout`, `#pageHardware`, etc.) handled client-side

### Deferred Ideas (OUT OF SCOPE)
- Payment processing (LemonSqueezy, Stripe)
- Analytics activation (Plausible)
- Video embed (awaiting URL)
</user_constraints>

---

## Summary

This phase covers four concrete tasks: rename the git branch, create the `.gitignore` and `vercel.json` files, connect the repo to Vercel, and wire up the custom domain with DNS. All tasks are well-documented by Vercel's official CLI and dashboard docs — no third-party tooling or guesswork required.

Vercel's Hobby (free) plan fully supports custom domains, automatic HTTPS, and preview deployments on every git push. No paid plan is required. The Hobby plan includes 100 GB bandwidth and 50 custom domains per project.

The most important nuance for this site: because there are no separate HTML files for `/about`, `/hardware`, etc., Vercel rewrites are required to serve `index.html` for those clean URL paths. The `cleanUrls` option is NOT appropriate here (it strips `.html` extensions from existing files, not the same thing). Instead, explicit `rewrites` entries in `vercel.json` are the correct mechanism.

**Primary recommendation:** Complete git branch rename first, then commit `vercel.json` + `.gitignore`, then connect to Vercel, then add domain. Order matters — Vercel's production branch default is `main`, so renaming before connecting avoids any reconfiguration.

---

## Area 1: Vercel Free Tier Capabilities

**Confidence: HIGH** — Verified against [Vercel plans docs](https://vercel.com/docs/plans) and [Vercel for GitHub docs](https://vercel.com/docs/git/vercel-for-github).

### What Hobby (free) plan includes

| Feature | Included on Free | Notes |
|---------|-----------------|-------|
| Custom domains | Yes — up to 50 per project | No paid plan needed |
| Automatic HTTPS/SSL | Yes — provisioned automatically | Certificate auto-renews |
| Preview deployments | Yes — every git push on every branch | Unique `*.vercel.app` URL per commit |
| Preview URLs on PRs | Yes — bot comment with link | Works on GitHub |
| 100 GB bandwidth (Fast Data Transfer) | Yes | Per month; site paused if exceeded |
| CI/CD (auto-deploy on push) | Yes | Webhook-based, instant |

### What is NOT on free tier

- Team members / organization collaboration (Pro only)
- Advanced analytics beyond basic insights
- Priority support

### Verdict

Everything Phase 05 needs — custom domain, HTTPS, preview URLs — is fully available on Hobby (free). No upgrade required.

---

## Area 2: Connecting GitHub Repo to Vercel (No Framework)

**Confidence: HIGH** — Verified against [Vercel for GitHub docs](https://vercel.com/docs/git/vercel-for-github) and [Vercel Git docs](https://vercel.com/docs/git).

### Dashboard steps (one-time setup)

1. Go to [vercel.com/new](https://vercel.com/new)
2. Click **"Add New Project"** → **"Import Git Repository"**
3. Authorize Vercel GitHub app if not already done (grant access to `dimea-website` repo specifically — no need for all-repo access)
4. Select `dimea-website` from the list
5. Configure project settings on the import screen:
   - **Framework Preset:** `Other` (set explicitly — this disables all build defaults)
   - **Root Directory:** leave as `.` (repo root)
   - **Build Command:** leave empty (no build step)
   - **Output Directory:** `.` (repo root — the HTML files are here)
   - **Install Command:** leave empty
6. Click **"Deploy"**

> **Note on framework null:** In `vercel.json`, `"framework": null` is the JSON equivalent of selecting "Other" in the UI. The UI setting takes precedence over `vercel.json` if both are set. No need to set this in `vercel.json` for a plain static site — the dashboard setting is sufficient.

### What happens automatically

- First push to `main` → production deployment (assigned to custom domain once configured)
- Any push to any other branch → preview deployment with unique URL
- Vercel bot comments on PRs with the preview URL

### Production branch

Vercel defaults to `main` as the production branch. Since the repo will be renamed before connecting, this requires no additional configuration.

---

## Area 3: Git Branch Rename (master → main)

**Confidence: HIGH** — Standard git operations, verified against [git-tower guide](https://www.git-tower.com/learn/git/faq/git-rename-master-to-main) and community documentation.

### Complete command sequence

```bash
# Step 1: Ensure you are on master
git checkout master

# Step 2: Rename the local branch
git branch -m master main

# Step 3: Push the new main branch to remote and set upstream tracking
git push -u origin main

# Step 4: Update the remote HEAD reference
git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main

# Step 5: Delete the old remote master branch
git push origin --delete master
```

### Step after git commands

Change the default branch on GitHub:

1. Go to the repo on GitHub → **Settings** → **Branches**
2. Under "Default branch", click the pencil icon
3. Select `main` → click **Update**
4. Confirm the warning

### Order requirement

Do this BEFORE connecting to Vercel. If Vercel is connected first with `master` as default, you would need to reconfigure the production branch in Vercel project settings afterward.

---

## Area 4: vercel.json — URL Rewrites for Static Site

**Confidence: HIGH** — Verified against [Vercel rewrites docs](https://vercel.com/docs/rewrites) and [vercel.json configuration reference](https://vercel.com/docs/project-configuration/vercel-json).

### How Vercel routing works for static files

Vercel serves static files first, before evaluating rewrites. This means:
- `/index.html` → served directly (no rewrite needed)
- `/privacy.html`, `/terms.html`, `/refund.html`, `/manual.html` → served directly
- `/about` → no matching file → falls through to rewrites → served as `/index.html`

This ordering is the correct behavior for this site.

### What NOT to use: cleanUrls

`cleanUrls: true` strips `.html` extensions from existing files (e.g. `privacy.html` becomes accessible at `/privacy`). It does NOT create routes for paths that have no corresponding file. Do NOT use this — it would break direct `.html` file access for the legal pages and manual without the extension.

### Correct vercel.json

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "rewrites": [
    { "source": "/about",       "destination": "/index.html" },
    { "source": "/hardware",    "destination": "/index.html" },
    { "source": "/impressions", "destination": "/index.html" },
    { "source": "/shop",        "destination": "/index.html" },
    { "source": "/app",         "destination": "/index.html" }
  ]
}
```

### Why exact paths instead of wildcard

A wildcard `/(.*) → /index.html` would intercept ALL unmatched paths, including future pages or API routes. Explicit paths are safer, more readable, and precisely match the five defined clean URLs. If a path is not in the list, Vercel returns its default 404 — which is the desired behavior.

### Trailing slash behavior

By default, Vercel does NOT add trailing slashes. Visiting `/about/` will NOT match the rewrite for `/about`. If trailing slash variants are needed, add them explicitly or add `"trailingSlash": false` (already the default). For this site, trailing slashes on these paths are not a concern — links are generated by the client without trailing slashes.

### Schema autocomplete

The `$schema` line provides IDE autocomplete/validation. Include it — it costs nothing and catches config errors early.

---

## Area 5: Custom Domain DNS Configuration

**Confidence: HIGH** — Verified against [Vercel add-a-domain docs](https://vercel.com/docs/domains/working-with-domains/add-a-domain) and [deploying-and-redirecting docs](https://vercel.com/docs/domains/deploying-and-redirecting).

### DNS record values

| Record type | Host / Name | Value | Purpose |
|-------------|-------------|-------|---------|
| `A` | `@` (or blank, depending on registrar) | `76.76.21.21` | Apex domain (`dimea.io`) → Vercel |
| `CNAME` | `www` | `cname.vercel-dns-0.com` | `www.dimea.io` → Vercel |

> **Important:** The CNAME target `cname.vercel-dns-0.com` is Vercel's general-purpose value. After adding the domain in the Vercel dashboard, run `vercel domains inspect dimea.io` to confirm the exact value shown — it may be a project-specific subdomain like `d1d4fc829fe7bc7c.vercel-dns-017.com`. Use whatever the dashboard shows.

> **Registrar note:** The `@` symbol means "the root/apex domain" in most registrar UIs. Some registrars label it "blank" or leave the Name field empty for the apex A record.

### Propagation time

DNS changes typically propagate within minutes to an hour for most registrars, though up to 48 hours is technically possible. Vercel polls for the records and updates the domain status automatically.

### Dashboard steps to add the domain

1. In your Vercel project → **Settings** → **Domains**
2. Click **Add Domain** → type `dimea.io` → **Add**
3. Vercel will prompt to also add `www.dimea.io` — accept it
4. Vercel shows the required DNS records (A record for apex, CNAME for www)
5. Add those records in your registrar's DNS panel
6. Wait for propagation — Vercel dashboard updates the status to "Valid Configuration"

### Configuring the www → apex redirect

Once both `dimea.io` and `www.dimea.io` are added to the project:

1. In **Settings** → **Domains**, find `www.dimea.io`
2. Click **Edit** on the `www.dimea.io` row
3. In the **"Redirect to"** dropdown, select `dimea.io`
4. Save

This configures a 308 permanent redirect from `www.dimea.io` to `dimea.io`. Vercel handles this at the CDN level — no `vercel.json` redirect entry required.

### HTTPS

SSL certificate is provisioned automatically by Vercel within minutes of DNS verification. No manual certificate management required. Covers both `dimea.io` and `www.dimea.io`.

---

## Area 6: .gitignore for Plain HTML Project

**Confidence: HIGH** — Standard, stable entries, consistent across all sources.

Per the locked decision: OS/editor junk only. `.planning/` and `.claude/` stay in git intentionally.

```gitignore
# macOS
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes

# Windows
Thumbs.db
Thumbs.db:encryptable
ehthumbs.db
ehthumbs_vista.db
desktop.ini
$RECYCLE.BIN/

# Editor
.vscode/
.idea/
*.swp
*.swo
*~
```

### What NOT to include

- `node_modules/` — no Node in this project
- `.env` — no environment variables
- `/dist`, `/build` — no build output
- `.planning/` — intentionally tracked (per locked decision)
- `.claude/` — intentionally tracked (per locked decision)

---

## Architecture Patterns

### Recommended file creation order

```
1. git branch -m master main + push       # Before Vercel connect
2. Create .gitignore                       # Commit
3. Create vercel.json                      # Commit
4. git push origin main                    # Triggers first deploy
5. Connect repo in Vercel dashboard        # One-time
6. Add domain in Vercel project settings   # One-time
7. Add DNS records at registrar            # One-time
8. Configure www → apex redirect           # One-time
```

### Repo state after this phase

```
dimea-website/
├── index.html
├── privacy.html
├── terms.html
├── refund.html
├── manual.html
├── vercel.json          ← new
├── .gitignore           ← new
├── .planning/           ← tracked intentionally
└── .claude/             ← tracked intentionally
```

---

## Common Pitfalls

### Pitfall 1: Connecting Vercel before renaming the branch

**What goes wrong:** Vercel connects with `master` as the production branch. After renaming to `main`, production deploys stop triggering because Vercel is still watching `master`.

**How to avoid:** Complete the full branch rename sequence (local + remote + GitHub default branch setting) BEFORE importing the repo in Vercel.

**Fix if it happens:** In Vercel project → Settings → Git → change "Production Branch" to `main`.

---

### Pitfall 2: Using cleanUrls instead of rewrites

**What goes wrong:** `cleanUrls: true` strips `.html` from existing files. It does NOT create routes for paths like `/about` that have no `.about.html` file. The paths would still 404.

**How to avoid:** Use `rewrites` in `vercel.json` (as documented in Area 4). Do not add `cleanUrls`.

---

### Pitfall 3: Wildcard rewrite breaks existing HTML pages

**What goes wrong:** Using `{ "source": "/(.*)", "destination": "/index.html" }` rewrites ALL unmatched paths including future ones, and may interfere with Vercel's own routing for `.well-known` paths or function routes.

**How to avoid:** List each path explicitly in the rewrites array. Five entries is not burdensome for five clean URLs.

---

### Pitfall 4: Wrong CNAME target for www

**What goes wrong:** Using `cname.vercel.com` (old value) or guessing the CNAME. Each project may have a project-specific CNAME value.

**How to avoid:** After adding the domain in the Vercel dashboard, read the exact value from the dashboard UI or from `vercel domains inspect dimea.io`. Do not hardcode without checking.

---

### Pitfall 5: Trailing slash rewrite mismatch

**What goes wrong:** A user visits `dimea.io/about/` (with trailing slash). The rewrite for `/about` does not match — returns 404.

**How to avoid:** For this site, links are generated in JavaScript without trailing slashes, so this is low risk. If needed, add duplicate rewrite entries with trailing slash variants, or investigate `trailingSlash` normalization in vercel.json.

---

### Pitfall 6: Forgetting to push vercel.json before Vercel connects

**What goes wrong:** Vercel's first deployment runs without `vercel.json` in the repo. The clean URLs work until someone visits `/about` directly and gets a 404.

**How to avoid:** Commit both `vercel.json` and `.gitignore` BEFORE connecting the repo in Vercel, so the first deployment already has correct routing.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| HTTPS | Manual cert management | Vercel automatic SSL | Auto-renews, covers all domains |
| www → apex redirect | `vercel.json` redirects | Vercel dashboard domain redirect | Handled at CDN layer, cached in browsers |
| Deploy CI/CD | GitHub Actions workflow | Vercel Git integration | Zero config, webhook-based, instant |
| Preview URLs | Manual staging env | Vercel preview deployments | Built in on every branch push |

---

## Code Examples

### Complete vercel.json (production-ready)

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "rewrites": [
    { "source": "/about",       "destination": "/index.html" },
    { "source": "/hardware",    "destination": "/index.html" },
    { "source": "/impressions", "destination": "/index.html" },
    { "source": "/shop",        "destination": "/index.html" },
    { "source": "/app",         "destination": "/index.html" }
  ]
}
```

Source: [Vercel rewrites docs](https://vercel.com/docs/rewrites) + [vercel.json reference](https://vercel.com/docs/project-configuration/vercel-json)

---

### Complete .gitignore

```gitignore
# macOS
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes

# Windows
Thumbs.db
Thumbs.db:encryptable
ehthumbs.db
ehthumbs_vista.db
desktop.ini
$RECYCLE.BIN/

# Editor
.vscode/
.idea/
*.swp
*.swo
*~
```

---

### Full git branch rename sequence

```bash
git checkout master
git branch -m master main
git push -u origin main
git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main
git push origin --delete master
```

Source: [git-tower guide](https://www.git-tower.com/learn/git/faq/git-rename-master-to-main)

---

### Vercel CLI domain setup (alternative to dashboard)

If using the CLI instead of the dashboard:

```bash
# Install CLI (if not already installed)
npm install -g vercel

# Link the project to Vercel
vercel link

# Add apex domain
vercel domains add dimea.io dimea-website

# Add www subdomain
vercel domains add www.dimea.io dimea-website

# Inspect to get exact DNS records
vercel domains inspect dimea.io

# Verify after adding DNS records at registrar
vercel domains inspect dimea.io

# Check SSL certificate
vercel certs ls
```

Source: [Vercel set-up custom domain docs](https://vercel.com/docs/domains/set-up-custom-domain)

---

## State of the Art

| Old Approach | Current Approach | Impact |
|--------------|------------------|--------|
| Heroku free tier for static sites | Vercel Hobby (free) | Vercel has no sleep/spin-up delay; instant deploys |
| Manual deploy FTP | Git push → auto-deploy | Zero manual steps after initial setup |
| Manual SSL (Certbot/Let's Encrypt) | Vercel automatic SSL | Certificate management fully automated |
| Separate staging server | Vercel preview deployments | Every branch gets a unique preview URL automatically |

---

## Open Questions

1. **Exact CNAME value for www.dimea.io**
   - What we know: Vercel's general-purpose value is `cname.vercel-dns-0.com`; project-specific values may differ
   - What's unclear: The exact value until the domain is added to the project in the dashboard
   - Recommendation: Read it from the Vercel dashboard after adding the domain, not from this document

2. **Registrar-specific DNS UI**
   - What we know: All registrars support A and CNAME records; field names vary (some use "@" for apex, some leave blank)
   - What's unclear: The specific registrar UI since registrar is TBD
   - Recommendation: Plan should note both naming conventions ("@" and blank) for the apex A record

3. **DNS propagation timing**
   - What we know: Vercel verifies automatically; usually minutes to 1 hour
   - What's unclear: Whether dimea.io's TLD (.io) has any unusual TTL behavior
   - Recommendation: Not a blocker — plan should include a "wait and verify" step, not a hard timing dependency

---

## Validation Architecture

> `workflow.nyquist_validation` not set to false — validation section included.

### Test Framework

This phase has no automated tests in the traditional sense. Verification is operational (DNS propagation, HTTPS, HTTP responses) rather than unit/integration tested.

| Property | Value |
|----------|-------|
| Framework | Manual verification + CLI checks |
| Config file | none |
| Quick run command | `curl -I https://dimea.io` |
| Full suite command | See Phase Gate checklist below |

### Phase Requirements → Verification Map

| Req | Behavior | Test Type | Verification Command |
|-----|----------|-----------|---------------------|
| REQ-01 | `master` branch no longer exists on remote | manual | `git branch -r` — should show `origin/main` only |
| REQ-02 | Production deploy from `main` triggers automatically | smoke | Push a commit to `main`, check Vercel dashboard for new deployment |
| REQ-03 | Preview URL generated on non-main branch push | smoke | Push to any other branch, check Vercel dashboard |
| REQ-04 | `dimea.io` resolves and serves `index.html` | smoke | `curl -I https://dimea.io` — expect `200 OK` |
| REQ-05 | `www.dimea.io` redirects to `dimea.io` | smoke | `curl -I https://www.dimea.io` — expect `308` redirect to `https://dimea.io` |
| REQ-06 | HTTPS on apex domain | smoke | `curl -I https://dimea.io` — connection succeeds without cert error |
| REQ-07 | Clean URL `/about` serves index.html | smoke | `curl -I https://dimea.io/about` — expect `200 OK` |
| REQ-08 | Clean URL `/hardware` serves index.html | smoke | `curl -I https://dimea.io/hardware` — expect `200 OK` |
| REQ-09 | Clean URL `/impressions` serves index.html | smoke | `curl -I https://dimea.io/impressions` — expect `200 OK` |
| REQ-10 | Clean URL `/shop` serves index.html | smoke | `curl -I https://dimea.io/shop` — expect `200 OK` |
| REQ-11 | Clean URL `/app` serves index.html | smoke | `curl -I https://dimea.io/app` — expect `200 OK` |
| REQ-12 | `privacy.html`, `terms.html`, `refund.html` serve directly | smoke | `curl -I https://dimea.io/privacy.html` — expect `200 OK` |
| REQ-13 | Repo is clean (no untracked OS junk) | manual | `git status` — only tracked files |

### Phase Gate

All of the following must be green before closing this phase:

- [ ] `git branch -r` shows `origin/main`, no `origin/master`
- [ ] GitHub default branch shows `main`
- [ ] `vercel.json` and `.gitignore` committed and visible on `main`
- [ ] Vercel project dashboard shows deployment from `main` as "Production"
- [ ] `curl -I https://dimea.io` → `HTTP/2 200`
- [ ] `curl -I https://www.dimea.io` → `308` redirect to `https://dimea.io`
- [ ] `curl -I https://dimea.io/about` → `200`
- [ ] `curl -I https://dimea.io/hardware` → `200`
- [ ] `curl -I https://dimea.io/impressions` → `200`
- [ ] `curl -I https://dimea.io/shop` → `200`
- [ ] `curl -I https://dimea.io/app` → `200`
- [ ] `curl -I https://dimea.io/privacy.html` → `200`

---

## Sources

### Primary (HIGH confidence)
- [Vercel plans / Hobby tier](https://vercel.com/docs/plans) — confirmed custom domains, HTTPS, preview deployments on free tier
- [Vercel for GitHub integration](https://vercel.com/docs/git/vercel-for-github) — confirmed preview URL behavior, production branch logic
- [Vercel rewrites docs](https://vercel.com/docs/rewrites) — confirmed `source`/`destination` syntax, static file precedence
- [vercel.json configuration reference](https://vercel.com/docs/project-configuration/vercel-json) — confirmed `cleanUrls` behavior, `rewrites` schema, `framework: null` for "Other"
- [Vercel set-up custom domain](https://vercel.com/docs/domains/set-up-custom-domain) — exact CLI commands, A record IP `76.76.21.21`, CNAME `cname.vercel-dns-0.com`
- [Vercel adding & configuring custom domain](https://vercel.com/docs/domains/working-with-domains/add-a-domain) — dashboard steps, apex vs subdomain DNS record types
- [Vercel deploying & redirecting domains](https://vercel.com/docs/domains/deploying-and-redirecting) — www → apex redirect via dashboard, DNS spec note on CNAME vs A

### Secondary (MEDIUM confidence)
- [git-tower: rename master to main](https://www.git-tower.com/learn/git/faq/git-rename-master-to-main) — `git branch -m` sequence verified against standard git documentation

### Tertiary (LOW confidence)
- None — all critical findings verified against official Vercel docs

---

## Metadata

**Confidence breakdown:**
- Free tier capabilities: HIGH — read directly from official Vercel plans page
- Vercel project setup steps: HIGH — from official Git integration and project docs
- vercel.json rewrite syntax: HIGH — from official vercel.json reference and rewrites docs
- DNS record values: HIGH — from official domain setup docs (note CNAME may be project-specific)
- git branch rename: HIGH — standard git commands, widely documented
- .gitignore contents: HIGH — stable, OS-standard entries

**Research date:** 2026-03-14
**Valid until:** 2026-09-14 (Vercel platform docs are stable; DNS record IPs very rarely change)
