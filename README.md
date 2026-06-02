# UMANG App — Reviews Dashboard

An interactive React dashboard for the 18,600 Google Play Store reviews of the
**UMANG** app (`in.gov.umang.negd.g2c`).

It is **build-free**: a single `index.html` loads React 18, Chart.js and Tailwind
from CDNs and reads a pre-computed `data.json`. No Node, npm, or bundler needed —
perfect for GitHub Pages.

## Features

- **RAG classification by star rating**
  - 🟢 **Green** = 4–5 ★
  - 🟠 **Amber** = 2–3 ★
  - 🔴 **Red**   = 0–1 ★
- **Filters for every criterion** — RAG chips, individual star toggles, theme,
  free-text search, minimum-sentiment slider, and a "has developer reply" toggle.
- **Dark / Light mode** toggle (remembered in `localStorage`).
- **KPIs** — total reviews, **average rating** (/5), **average sentiment** (/10),
  and Green/Amber/Red counts with percentages.
- **Six charts** — RAG doughnut, rating distribution, sentiment histogram,
  avg-rating-&-sentiment trend over time, top themes, and RAG mix over time.
- **Tooltips on every visual** — hover the small **ⓘ** on each KPI and chart to
  read exactly how that number/visual is calculated.
- **Export filtered CSV** — downloads exactly the rows currently in view.

## How the numbers are calculated

| Metric | Calculation |
|--------|-------------|
| **Avg Rating** | Mean of star ratings (1–5) across filtered reviews: `Σ(rating) ÷ n` |
| **Avg Sentiment** | Each review's text scored with **VADER** (compound, −1…+1), rescaled to 0–10 via `(compound + 1) × 5` (so 5 = neutral), then averaged over the filtered set |
| **RAG** | Bucketed **only by star rating**: 4–5 → Green, 2–3 → Amber, 0–1 → Red |
| **Themes** | Keyword-dictionary match on review text (OTP/Login, Server/Outage, App crash, EPFO/PF, …); a review can have several |

## Files

```
index.html      The whole dashboard (React + Chart.js + Tailwind via CDN)
data.json       Pre-computed reviews + meta (built by build_data.py)
build_data.py   Regenerates data.json from ../umang_reviews_archive.csv
```

## Run locally

```powershell
# from this folder
python -m http.server 8000
# then open http://localhost:8000
```

(Must be served over HTTP — opening `index.html` from `file://` blocks the
`fetch("data.json")` call.)

## Refresh the data

```powershell
# from the project root (.venv has pandas + vaderSentiment)
.\.venv\Scripts\python.exe .\umang-dashboard\build_data.py
```

This re-reads `../umang_reviews_archive.csv` (kept up to date by the weekly
scraper) and rewrites `data.json`. Re-upload `data.json` to publish the refresh.

## Deploy to GitHub Pages

See **DEPLOY.md** for click-by-click steps.
