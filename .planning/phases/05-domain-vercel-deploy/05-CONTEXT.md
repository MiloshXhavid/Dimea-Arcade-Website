# Phase 05 Context — Domain + Vercel Deploy

**Phase goal:** Get the site live on a real domain with CI/CD deploy previews.
**Status:** Ready for research + planning

---

## Decisions

### Domain

- **Chosen domain:** `dimea.io`
- **Registration:** 3-year term (~CHF 150)
- **Registrar:** TBD — plan should cover DNS steps generically for any registrar
- **Canonical:** apex (`dimea.io`) is canonical; `www.dimea.io` redirects to apex (Vercel handles automatically when both are added as domains)

### Repository

- **Branch rename:** `master` → `main` before Vercel connect
- **Deploy trigger:** Vercel production deploys on push to `main`; preview URLs on all other branches
- **Solo dev:** no branch protection rules needed
- **`.gitignore`:** OS/editor junk only (`.DS_Store`, `Thumbs.db`, `desktop.ini`). `.planning/` and `.claude/` stay in git intentionally.

### Vercel Config

- **Project root:** repo root (flat HTML, no build step, no framework preset — set to "Other")
- **Output directory:** `.` (repo root)
- **Custom 404:** plain Vercel default — no custom 404 page needed
- **`manual.html`:** unlisted only (already `noindex` in meta) — no Vercel-level access restriction needed

### URL Rewrites

Clean URLs for all site sections via `vercel.json` rewrites. Each path serves `index.html` so the client-side scroll/hash takes over:

| Clean URL | Destination |
|-----------|-------------|
| `/about` | `/index.html` |
| `/hardware` | `/index.html` |
| `/impressions` | `/index.html` |
| `/shop` | `/index.html` |
| `/app` | `/index.html` |

Anchors (`#pageAbout`, `#pageHardware`, etc.) are handled client-side — rewrites just ensure the HTML shell loads from any path.

---

## Code Context

### HTML files in repo root
- `index.html` — main site (all sections: Home, About, Hardware, Impressions, Shop, App)
- `privacy.html`, `terms.html`, `refund.html` — legal pages
- `manual.html` — internal reference, `noindex`

### Section anchors in index.html
- `#pageHome`, `#pageAbout`, `#pageHardware`, `#pageImpressions`, `#pageShop`, `#pageApp`

### No existing Vercel config
- No `vercel.json` — needs to be created
- No `.gitignore` — needs to be created

---

## Out of Scope (deferred)

- Payment processing (LemonSqueezy, Stripe) — future phase
- Analytics activation (Plausible) — future phase
- Video embed (awaiting YouTube/Vimeo URL)
