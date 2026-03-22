---
phase: 05-domain-vercel-deploy
plan: 02
subsystem: deployment / dns
tags: [vercel, dns, domain, https, ci-cd]
completed: 2026-03-22
---

## What Was Done

Connected the dimea-website repo to Vercel and made dimea.io live with HTTPS, CI/CD, and clean URL routing.

### Steps completed
- Imported dimea-website repo into Vercel project (dimea-arcade-website), framework set to Other, output directory `.`
- Added `dimea.io` as primary domain and `www.dimea.io` as redirect in Vercel project settings
- Configured Vercel nameservers (`ns1.vercel-dns.com`) at registrar — Vercel manages DNS
- Set A record `@ → 76.76.21.21` in Vercel DNS panel (later updated to `216.198.79.1` per Vercel IP expansion recommendation)
- Set `dimea.io` as primary domain in project settings so `www.dimea.io` → 307 → `dimea.io`
- Let's Encrypt cert auto-provisioned by Vercel, covering `DNS:dimea.io` and `DNS:*.dimea.io`

### Vercel project
- Production URL: https://dimea-arcade-website.vercel.app
- Custom domain: https://dimea.io
- Every push to `main` → production deploy
- Every push to non-main branch → preview URL

## Verification Results

| Check | Result |
|-------|--------|
| `https://dimea.io` | ✅ 200 OK |
| `https://www.dimea.io` | ✅ 307 → `https://dimea.io` |
| HTTPS cert | ✅ Let's Encrypt, covers apex + wildcard |
| `/about` clean URL | ✅ 200 |
| `/hardware` clean URL | ✅ 200 |
| `/impressions` clean URL | ✅ 200 |
| `/shop` clean URL | ✅ 200 |
| `/app` clean URL | ✅ 200 |
| `/privacy.html` | ✅ 200 |
| Push to main → deploy | ✅ |
| `git branch -r` | ✅ `origin/main` only |

## Notes

- During setup, the apex A record initially pointed to `3.33.255.208` (wrong server running Caddy), causing "Invalid Configuration" in Vercel. Fixed by updating to `76.76.21.21`, then again to `216.198.79.1` per Vercel's IP expansion recommendation.
- Windows schannel TLS library blocked local HTTPS verification to apex — used openssl and redirect-following curl to confirm cert and routing were correct.
- DNS TTL is 60s so propagation was near-instant after each record change.
