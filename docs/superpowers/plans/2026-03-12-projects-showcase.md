# Projects Showcase Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Set up a static-file monorepo on Vercel where each root-level folder is a self-contained client showcase served at a clean URL.

**Architecture:** Single Git repo with flat folder structure. Each client folder holds `index.html`, `assets/`, and `source/` (.pen files). Vercel serves everything as static files with clean URLs — no build step, no framework.

**Tech Stack:** Static HTML/CSS, Git/GitHub, Vercel (static hosting), Pencil (.pen design files)

**Spec:** `docs/superpowers/specs/2026-03-12-projects-showcase-design.md`

---

## File Structure

```
Projects Showcase/
├── sample-client/
│   ├── source/              ← .pen design files (placeholder)
│   ├── assets/
│   │   └── styles.css       ← minimal showcase styles
│   └── index.html           ← sample showcase page
├── docs/                    ← specs and plans (already exists)
├── .gitignore
├── vercel.json
└── README.md
```

---

## Chunk 1: Repo Setup and Sample Client

### Task 1: Create `.gitignore`

**Files:**
- Create: `.gitignore`

- [ ] **Step 1: Create `.gitignore`**

```
# OS files
.DS_Store
Thumbs.db
Desktop.ini

# Editor files
*.swp
*.swo
*~
.vscode/
.idea/

# Logs
*.log

# Node (in case Vercel CLI is used locally)
node_modules/

# Keep .pen files — they are design source
```

- [ ] **Step 2: Verify file exists**

Run: `cat .gitignore`
Expected: contents shown above

---

### Task 2: Create `vercel.json`

**Files:**
- Create: `vercel.json`

- [ ] **Step 1: Create `vercel.json`**

```json
{
  "cleanUrls": true,
  "trailingSlash": false
}
```

- [ ] **Step 2: Verify file exists**

Run: `cat vercel.json`
Expected: JSON with `cleanUrls` and `trailingSlash` settings

---

### Task 3: Create `README.md`

**Files:**
- Create: `README.md`

- [ ] **Step 1: Create `README.md`**

```markdown
# Projects Showcase

Static HTML client showcases deployed to Vercel.

## Structure

Each root-level folder is a client showcase:

```
client-name/
├── source/       .pen design files
├── assets/       images, css, fonts
└── index.html    the showcase page
```

## Adding a new showcase

1. Create a new folder at the root (lowercase kebab-case, e.g., `acme-corp`)
2. Add `index.html`, `assets/`, and `source/` inside it
3. Use relative paths for all assets (e.g., `assets/hero.jpg`)
4. Push to main — Vercel auto-deploys
5. Share: `projects-showcase.vercel.app/{folder-name}`

## URL

Each folder is served at `projects-showcase.vercel.app/{folder-name}`
```

- [ ] **Step 2: Verify file exists**

Run: `cat README.md`
Expected: README contents shown above

- [ ] **Step 3: Commit repo config files**

```bash
git add .gitignore vercel.json README.md
git commit -m "chore: add repo config — .gitignore, vercel.json, README"
```

---

### Task 4: Create sample client showcase

**Files:**
- Create: `sample-client/assets/styles.css`
- Create: `sample-client/index.html`
- Create: `sample-client/source/.gitkeep`

- [ ] **Step 1: Create sample client directory structure**

```bash
mkdir -p sample-client/source sample-client/assets
```

- [ ] **Step 2: Create `sample-client/source/.gitkeep`**

Empty file — keeps the `source/` folder in git until real `.pen` files are added.

- [ ] **Step 3: Create `sample-client/assets/styles.css`**

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: system-ui, -apple-system, sans-serif;
  color: #1a1a1a;
  background: #fafafa;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.showcase {
  max-width: 720px;
  padding: 3rem 2rem;
  text-align: center;
}

.showcase h1 {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  letter-spacing: -0.02em;
}

.showcase p {
  font-size: 1.125rem;
  line-height: 1.6;
  color: #555;
}
```

- [ ] **Step 4: Create `sample-client/index.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sample Client — Projects Showcase</title>
  <link rel="stylesheet" href="assets/styles.css">
</head>
<body>
  <main class="showcase">
    <h1>Sample Client</h1>
    <p>This is a sample showcase page. Replace this with the actual client design exported from Pencil.</p>
  </main>
</body>
</html>
```

- [ ] **Step 5: Verify the HTML references assets with relative paths**

Run: `grep -c "assets/" sample-client/index.html`
Expected: 1 (the stylesheet link)

- [ ] **Step 6: Commit sample client**

```bash
git add sample-client/
git commit -m "feat: add sample-client showcase folder"
```

---

### Task 5: Commit docs and push

**Files:**
- Existing: `docs/` folder (specs + plans)

- [ ] **Step 1: Add docs to git**

```bash
git add docs/
git commit -m "docs: add project spec and implementation plan"
```

- [ ] **Step 2: Push everything to GitHub**

```bash
git branch -M main
git push -u origin main
```

Expected: all commits pushed to `https://github.com/alexprime007-coder/Projects-Showcase.git`

---

### Task 6: Connect to Vercel

- [ ] **Step 1: Deploy to Vercel**

Use Vercel CLI or connect via Vercel dashboard:
- Go to vercel.com → Import Git Repository
- Select `alexprime007-coder/Projects-Showcase`
- Framework: **Other** (no framework)
- Build command: leave empty
- Output directory: leave empty (`.` — serve repo root)
- Deploy

- [ ] **Step 2: Verify deployment**

Visit: `projects-showcase.vercel.app/sample-client`
Expected: the sample showcase page renders with styled text

- [ ] **Step 3: Verify clean URLs work**

Visit: `projects-showcase.vercel.app/sample-client` (without `.html`)
Expected: same page loads correctly
