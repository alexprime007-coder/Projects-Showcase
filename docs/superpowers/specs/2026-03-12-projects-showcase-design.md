# Projects Showcase — Design Spec

**Date:** 2026-03-12
**Status:** Approved

## Overview

A single repository that hosts multiple static HTML client showcases. Each client gets their own folder at the repo root containing a landing page and assets. The repo deploys to Vercel as a single project, giving each client a clean preview URL.

**Purpose:** Quickly showcase website designs to clients for approval before building the full production site.

## Requirements

- Single repo, single Vercel project
- Each client showcase is a root-level folder with self-contained HTML + assets
- Pencil (.pen) design source files stored alongside each showcase
- Auto-deploys on push to main branch
- No build step — pure static HTML
- No index/dashboard — direct links shared with clients
- Showcase-only — approved designs are built separately in other repos

## URL Structure

- **Base URL:** `projects-showcase.vercel.app`
- **Client URL pattern:** `projects-showcase.vercel.app/{client-name}`
- Custom domain can be added later (e.g., `showcase.yourstudio.com`)

## Folder Structure

```
Projects Showcase/
├── {client-name}/
│   ├── source/              .pen design files (design source, not served)
│   ├── index.html           the showcase landing page
│   └── assets/              images, css, fonts
├── docs/                    specs and project docs
├── .gitignore
├── vercel.json
└── README.md
```

### Rules

1. Every client folder sits at the repo root level
2. Client folder names MUST be lowercase kebab-case (e.g., `acme-corp`) — they become URL path segments
3. Every client folder MUST have an `index.html` — this is what Vercel serves
4. `source/` holds `.pen` files (kept in version control for design history)
5. `assets/` holds images, CSS, fonts referenced by `index.html`
6. All asset paths in HTML are relative (e.g., `assets/hero.jpg`), making each folder fully self-contained
7. Repo-level files (`.gitignore`, `vercel.json`, `README.md`) and the `docs/` folder are the only non-client items at root

## Vercel Configuration

**`vercel.json`:**
```json
{
  "cleanUrls": true,
  "trailingSlash": false
}
```

- `cleanUrls`: strips `.html` extensions for clean links
- `trailingSlash`: no trailing slashes in URLs
- No rewrites needed — Vercel natively serves `{folder}/index.html` at `/{folder}`
- No build command — static files served as-is

## Workflow

1. Design the client's landing page in Pencil
2. Export/build the HTML + assets into a new root-level folder named after the client
3. Store the `.pen` source files in the `source/` subfolder
4. `git add` + `git commit` + `git push` to main
5. Vercel auto-deploys
6. Share `projects-showcase.vercel.app/{client-name}` with the client
7. Client reviews and provides feedback
8. Iterate on the design (repeat steps 1-6)
9. Once approved, build the full production site in a separate repo

## Tech Stack

- **Design:** Pencil (.pen files)
- **Frontend:** Static HTML, CSS, images — no framework, no build
- **Hosting:** Vercel (free tier, auto-deploy from GitHub)
- **Version Control:** Git + GitHub

## What Gets Built (Implementation)

1. Initialize Git repo
2. Create `vercel.json` with clean URL config
3. Create `.gitignore` (system files, editor files; keep `.pen` files)
4. Create a sample client folder to validate the setup
5. Connect repo to Vercel
