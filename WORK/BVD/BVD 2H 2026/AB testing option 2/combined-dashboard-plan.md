# Combined Instana Dashboard — GitHub Pages Plan

## Overview

Create a single `instana-combined-dashboard.html` file that acts as a **persona switcher wrapper**. It displays a full-screen iframe pointing to one of three individual dashboard files. Switching personas swaps the iframe `src`. All three source files are uploaded to the same GitHub repo and served via GitHub Pages — giving the team one shareable URL.

**Source files: zero changes.**

---

## Workspace Context

The user will upload the **entire workspace folder** to an existing GitHub repo. All artefacts in the workspace must remain exactly where they are — no files moved, renamed, or deleted. The combined dashboard file will be created at the workspace root alongside the three source files.

**Current workspace root contents (all at the same level):**

```
AB testing/                              ← workspace root = repo root
├── instana-bvd-dashboard copy.html      ← Shani (unchanged)
├── instana-carlos-dashboard copy.html   ← Carlos (unchanged)
├── instana-rj-dashboard copy.html       ← RJ (unchanged)
├── combined-dashboard-plan.md           ← this plan (stays)
├── .vscode/settings.json                ← stays
└── instana-combined-dashboard.html      ← NEW — created here
```

Because all four HTML files are at the same folder level, relative paths in the combined file are simply the **bare filenames** (no path prefix needed).

---

## Architecture

```
GitHub Repo (uploaded as-is from workspace)
├── instana-bvd-dashboard copy.html     ← Shani (unchanged)
├── instana-carlos-dashboard copy.html  ← Carlos (unchanged)
├── instana-rj-dashboard copy.html      ← RJ (unchanged)
├── instana-combined-dashboard.html     ← NEW — the combined switcher
├── combined-dashboard-plan.md          ← plan artefact (stays)
└── .vscode/settings.json               ← stays
```

Each file is served as a static page by GitHub Pages. The combined file references the other three by **relative URL** (bare filename only) — so they always resolve correctly within the same repo/deployment, and locally via a simple HTTP server.

---

## Sub-Tasks

---

### Sub-Task 1 — Create `instana-combined-dashboard.html`

**Intent**  
Build a minimal, self-contained wrapper page that shows a full-screen `<iframe>` defaulting to Shani's dashboard. A floating persona switcher bar at the top lets users switch between the three dashboards.

**Expected Outcomes**  
- File exists at repo root as `instana-combined-dashboard.html`
- Opening the file in a browser shows Shani's dashboard in full-screen iframe
- Clicking Carlos in the switcher replaces the iframe src with Carlos's file
- Clicking RJ in the switcher replaces the iframe src with RJ's file
- Clicking Shani returns to Shani's file
- The active persona is visually indicated (highlighted button)
- No changes to any of the three source dashboard files

**Todo List**  
1. Create `instana-combined-dashboard.html` with `<!DOCTYPE html>` structure
2. Add `<style>` block:
   - `body, html { margin:0; padding:0; height:100%; overflow:hidden }`
   - `.switcher-bar` — slim top bar (40–48px) with IBM g100 dark background, flex layout, persona buttons, IBM Instana branding label
   - `.switcher-btn` — pill/tab styled buttons; active state uses persona accent colour
   - `iframe#dashboard-frame` — `position:fixed; top:48px; left:0; right:0; bottom:0; width:100%; height:calc(100vh - 48px); border:none`
3. Add `<body>`:
   - `.switcher-bar` containing:
     - IBM Instana wordmark/label (left)
     - Three persona buttons: `Shani — Executive`, `Carlos — SRE`, `RJ — DevOps` (centre or right)
     - Each button has `data-src` attribute pointing to the relative file path
   - `<iframe id="dashboard-frame">` with initial `src` pointing to `instana-bvd-dashboard copy.html`
4. Add `<script>`:
   - `switchTo(src, btn)` function — sets `iframe.src = src`, toggles `.active` class on buttons
   - On page load, mark Shani button as active
   - Button `onclick` attributes call `switchTo(this.dataset.src, this)`
5. Persona accent colours for active state:
   - Shani: `#a56eff`
   - Carlos: `#4589ff`
   - RJ: `#ff832b`

**Relevant Context**
- Source files: `instana-bvd-dashboard copy.html`, `instana-carlos-dashboard copy.html`, `instana-rj-dashboard copy.html`
- All three files are at the workspace root — same level as the combined file. Relative `src` values are bare filenames, e.g. `src="instana-bvd-dashboard copy.html"` (space and "copy" suffix must be preserved exactly, or URL-encoded as `instana-bvd-dashboard%20copy.html` in the iframe src attribute)
- The switcher bar should use Carbon g100 dark theme colours to match the dashboards visually: `background: #161616`, `color: #f4f4f4`
- Do not move, rename or restructure any existing file in the workspace

**Status**: [x] done

---

### Sub-Task 2 — Verify local file behaviour and relative paths

**Intent**  
Confirm the combined file works correctly when opened from the local file system before pushing to GitHub. Iframes with `file://` protocol can have cross-origin restrictions in some browsers — identify any issues.

**Expected Outcomes**  
- Combined file opens in Chrome/Safari and loads Shani's dashboard in the iframe
- Persona switcher correctly swaps iframe src
- If `file://` iframe loading is blocked in a specific browser, document the workaround (e.g. use a local server or push to GitHub Pages directly)

**Todo List**  
1. Open `instana-combined-dashboard.html` locally in the default browser
2. Verify each persona switch loads the correct dashboard
3. Note any browser console errors related to iframe cross-origin or mixed content
4. If `file://` blocks iframes: document that GitHub Pages is the intended environment and local testing requires `python3 -m http.server` or VS Code Live Server

**Relevant Context**  
- Chrome blocks `file://` iframe loads by default due to same-origin policy — this is expected and not a bug in the code
- The fix is always GitHub Pages (the intended deployment)

**Status**: [x] done — documented in file header comment and README

---

### Sub-Task 3 — Document the GitHub Pages setup steps

**Intent**  
Write a short `README.md` that explains how to push the files to GitHub and activate GitHub Pages, so any team member can re-deploy or share the URL.

**Expected Outcomes**  
- `README.md` exists at repo root
- README explains: which files to push, how to enable GitHub Pages (Settings → Pages → Deploy from branch → main → root), and the resulting shareable URL pattern
- README includes the one-line local test command (`python3 -m http.server`)

**Todo List**
1. Create `README.md` with sections: Overview, Files, Local Preview, GitHub Pages Setup, Team Sharing
2. Include the GitHub Pages URL pattern: `https://<username>.github.io/<repo-name>/instana-combined-dashboard.html`
3. Note that all four HTML files must be at the workspace root (repo root) for relative URLs to resolve
4. Note that the workspace is uploaded as-is — no restructuring required before pushing

**Relevant Context**
- User already has a GitHub repo — the full workspace folder will be uploaded to it
- README is for team members who receive the shareable URL or need to redeploy
- Filenames contain spaces ("copy") — GitHub Pages handles these correctly; relative href/src references should use the exact filename or URL-encode the space as `%20`

**Status**: [x] done

---

## Key Decisions

| Decision | Choice |
|---|---|
| Source files modified | No — zero changes |
| Inner shell visible | Yes — each dashboard shows its own full shell inside the iframe |
| Switcher bar position | Fixed slim top bar, 48px tall, sits above the iframe |
| Iframe sizing | `calc(100vh - 48px)` height, full width |
| File referencing | Bare filenames as relative paths — all files at repo root |
| Workspace upload | Entire workspace uploaded as-is — no files moved or renamed |
| Filenames with spaces | Use `%20` in iframe `src` attributes, e.g. `instana-bvd-dashboard%20copy.html` |
| GitHub Pages branch | `main`, root folder |
| Active persona on load | Shani |

---

## Shareable URL

Once GitHub Pages is enabled, the team URL will be:

```
https://<your-username>.github.io/<your-repo-name>/instana-combined-dashboard.html
```

All four files must be committed to the repo at the same folder level.
