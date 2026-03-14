---
phase: 05-domain-vercel-deploy
plan: 01
subsystem: git / deployment-config
tags: [git, vercel, deployment, spa-routing]
dependency_graph:
  requires: []
  provides: [vercel-rewrite-config, gitignore, main-branch]
  affects: [vercel-deploy, spa-routing]
tech_stack:
  added: [vercel.json]
  patterns: [SPA clean URL rewrites, explicit route mapping]
key_files:
  created:
    - .gitignore
    - vercel.json
  modified: []
decisions:
  - Local branch renamed master → main; remote push deferred (no remote configured)
  - No remote origin exists — user must create GitHub repo and add remote before pushing
  - vercel.json uses explicit rewrites (not wildcard) for /about, /hardware, /impressions, /shop, /app
  - .gitignore covers macOS + Windows + editor artifacts only; .planning/ and .claude/ intentionally retained
metrics:
  duration: ~5 minutes
  completed: 2026-03-14
---

# Phase 05 Plan 01: Git Setup + Vercel Config Summary

Local branch renamed to `main`, `.gitignore` and `vercel.json` committed — repo is ready for GitHub remote and Vercel deploy.

## What Was Done

### Task 1: Branch rename master → main

- Local branch renamed: `master` → `main` (via `git branch -m master main`)
- `git branch` confirms `* main` is the current local branch
- **Push blocked:** No `origin` remote is configured on this repo. The `git push -u origin main` and `git push origin --delete master` commands could not run.
- No per-task commit for this task (branch rename is a ref operation only, no file changes)

### Task 2: Create .gitignore

- Created `.gitignore` at repo root covering macOS, Windows, and editor artifacts
- Does NOT contain `.planning/`, `.claude/`, `node_modules/`, `.env`, or `/dist` — all intentionally excluded per plan
- `.planning/` and `.claude/` still appear as untracked in `git status` — confirmed not suppressed
- Commit: `1cf8d4f` — `chore(05-01): add .gitignore — OS/editor exclusions only`

### Task 3: Create vercel.json

- Created `vercel.json` at repo root with 5 explicit SPA route rewrites
- Routes covered: `/about`, `/hardware`, `/impressions`, `/shop`, `/app` — all rewrite to `/index.html`
- Legal pages (`/privacy.html`, `/terms.html`, `/refund.html`) intentionally excluded from rewrites — served as real files by Vercel
- Commit: `d340bd6` — `feat(05-01): add vercel.json — clean URL rewrites for SPA sections`
- **Push blocked:** Same reason — no `origin` remote configured

## Commits

| Hash      | Message                                                          | Task |
| --------- | ---------------------------------------------------------------- | ---- |
| `1cf8d4f` | chore(05-01): add .gitignore — OS/editor exclusions only         | 2    |
| `d340bd6` | feat(05-01): add vercel.json — clean URL rewrites for SPA sections | 3  |

## Manual Steps Required (User Action Needed)

Before connecting to Vercel, the user must complete these steps in order:

**1. Create GitHub repository**
- Go to https://github.com/new
- Create repo named `dimea-website` (or your preferred name)
- Do NOT initialize with README/gitignore (repo already has commits)

**2. Add remote and push**
```bash
git remote add origin https://github.com/YOUR_USERNAME/dimea-website.git
git push -u origin main
```

**3. Update GitHub default branch**
- GitHub Settings → Branches → Default branch
- Click pencil icon → select `main` → click Update → confirm warning
- This ensures Vercel picks `main` as the production branch automatically

**4. Delete remote master (if it exists after push)**
- Only needed if GitHub somehow created a `master` branch
- `git push origin --delete master`

## Deviations from Plan

### Deviation: Remote push blocked (no origin remote)

- **Found during:** Task 1 (push attempt)
- **Issue:** The plan assumes `origin` remote exists. The repo has no remote configured — `git remote` returned empty output.
- **Action taken:** Local branch rename completed (`master` → `main`). Both file commits (`1cf8d4f`, `d340bd6`) made locally. Push steps documented in Manual Steps above.
- **Impact:** No commits are on GitHub yet. Vercel connection cannot proceed until the user adds the remote and pushes.
- **Classification:** Human-action gate — requires user to create GitHub repo before automation can continue.

## Verification Results

```
git branch       → * main          (PASS)
git branch -r    → (empty)         (no remote — expected given above)
git log -2       → d340bd6, 1cf8d4f (PASS — both plan commits present)
git status       → .planning/ and .claude/ still visible (PASS — not suppressed)
vercel.json      → 5 rewrites, all destination /index.html (PASS)
.gitignore       → contains .DS_Store and Thumbs.db, no .planning (PASS)
```

## Self-Check

- [x] `.gitignore` exists at repo root — FOUND
- [x] `vercel.json` exists at repo root — FOUND
- [x] Commit `1cf8d4f` exists — FOUND
- [x] Commit `d340bd6` exists — FOUND
- [x] Local branch is `main` — CONFIRMED
