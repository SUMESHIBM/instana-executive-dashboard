# IBM Instana — Combined Dashboard (Option 2)

A single-file, self-contained IBM Instana dashboard prototype where one user — **Shani** — can switch between three persona views via a **"Dashboard view"** ghost button in the filter bar. No iframes, no external dependencies, works from `file://` or any static host.

---

## Deliverable

| File | Description |
|---|---|
| `instana-combined-option2.html` | **The file to open / share.** All three dashboards inlined in one self-contained HTML file |
| `instana-bvd-dashboard copy.html` | Source: Shani — Executive view |
| `instana-carlos-dashboard copy.html` | Source: Carlos — Management view |
| `instana-rj-dashboard copy.html` | Source: RJ — Operational view |
| `combined-dashboard-plan.md` | Internal planning notes (not for sharing) |

> Open `instana-combined-option2.html` directly in any modern browser — no server required.

---

## How to switch dashboards

1. Open `instana-combined-option2.html`
2. In the filter bar (below the page title), click the **"Executive view ▾"** ghost button
3. Select **Management view** (Carlos) or **Operational view** (RJ)
4. The entire page content switches — the button label updates to reflect the active view
5. All three views show **"Welcome to Instana, Shani!"** in the page header

---

## Dashboard views

| View | Persona | Content |
|---|---|---|
| Executive view | Shani | Business-level KPIs, Account status (5-column usage grid), Incident closure tile, AI assistant |
| Management view | Carlos | SRE-level SLO/SLA monitoring, trace panel, cost metrics |
| Operational view | RJ | Infrastructure and alert operations view |

---

## Interactions available (all views)

- **Scenario A / B toggle** — top-right `SCENARIO A` button switches the dashboard between a healthy baseline (A) and a P1 incident stress state (B); KPIs, AI card, and account status values all update
- **AI Chat panel** — click the chat icon to open the watsonx assistant; send suggestions or free-text messages
- **Export panel** — export data to CSV, PDF, or destinations such as Slack/email
- **Ingest panel** (Executive view) — raise a ticket from the data ingestion alert
- **Trace panel** (Management view) — drill into a specific service trace, ping / assign RJ
- **Service criticality filter** (Executive view) — filter the dashboard by service tier

---

## Account status card

The Account status card shows **5 subscription usage columns**, each with:

- Metric label (Standard MVS, Essential MVS, Data ingestion, Managed POPs, Add ons)
- Current / total value (e.g. `1330/1350 MVS`)
- Two-tone bar — **blue** = consumed by subscription · **grey** = remaining entitlement

In **Scenario B**, all bars turn amber and values show maximum usage to simulate a projected overage.

---

## Architecture

Everything is compiled into one file — no iframe, no network requests, no external assets.

```
instana-combined-option2.html
├── <head>
│   └── <style>  — merged CSS from all three dashboards
├── <body>
│   ├── #dash-panel-shani    — Executive view (hidden/visible via class)
│   ├── #dash-panel-carlos   — Management view
│   ├── #dash-panel-rj       — Operational view
│   └── <script>
│       ├── IIFE (Shani)     — scoped JS, exposes window.fn_shani
│       ├── IIFE (Carlos)    — scoped JS, exposes window.fn_carlos
│       ├── IIFE (RJ)        — scoped JS, exposes window.fn_rj
│       └── Combined switcher — switchDashView(), toggleDashViewMenu()
```

All three panels' JavaScript functions are isolated in IIFEs to prevent global scope collisions. A scoped `$p(id)` helper inside each IIFE queries elements only within that panel's `div`, eliminating duplicate-ID conflicts.

---

## GitHub Pages

This file is in the `WORK/BVD/BVD 2H 2026/AB testing option 2/` folder of the `instana-executive-dashboard` repo.

Once GitHub Pages is enabled on `main` (root), the shareable URL is:

```
https://sumeshibm.github.io/instana-executive-dashboard/WORK/BVD/BVD%202H%202026/AB%20testing%20option%202/instana-combined-option2.html
```

**To enable GitHub Pages:**
1. Go to the repo → **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Click **Save** — live in ~1 minute

---

## Local preview

The file opens directly from `file://` — just double-click it or drag it into a browser. No local server needed.

If you prefer a server:

```bash
cd "AB testing option 2"
python3 -m http.server 8080
# then open: http://localhost:8080/instana-combined-option2.html
```

---

## Colour reference

| Token | Hex | Used for |
|---|---|---|
| Carbon interactive | `#0f62fe` | Tab underlines, buttons, focus rings, progress bars |
| Shani persona | `#a56eff` | Shani avatar |
| Carlos persona | `#4589ff` | Carlos avatar |
| RJ persona | `#ff832b` | RJ avatar, "Ping RJ" button — intentional persona colour |

---

## Source files

The three source dashboard files (`instana-bvd-dashboard copy.html`, `instana-carlos-dashboard copy.html`, `instana-rj-dashboard copy.html`) are **read-only reference files**. All changes are made directly to `instana-combined-option2.html`.
