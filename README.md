# 🚦 Railway Crossing Predictor

A mobile-first web app that predicts when the **Kadubeesanahalli Railway Crossing** gate (Bengaluru) will open and close — built for commuters who are tired of guessing whether to wait or take a detour.

**Live app:** https://bxtgeek.github.io/railway-crossing-data/

---

## What it does

- Shows whether the crossing is currently **OPEN** or **CLOSED**
- Predicts the **next closure time** and **expected reopening time**
- Lists upcoming trains, their direction, and ETA to the crossing
- Merges overlapping trains into a single continuous closure window (no false "open → close → open" flickers)
- Shows a confidence score that reflects how fresh the underlying data is
- Works on a 5-minute refresh cycle with no backend server required

---

## How it works

- A **GitHub Action** polls the RailRadar live train API for two stations (KJM and BLRR) every 5 minutes
- It writes the results into a single `cache.json` file and commits it to the repo
- The **React app** fetches that file directly from `raw.githubusercontent.com`, so it always reads the latest data instantly — without needing a full site rebuild
- A **prediction engine** running entirely in the browser turns raw train timings into gate open/close windows

---

## Tech stack

| Layer | Tools used |
|---|---|
| Frontend | React, TypeScript, Vite, Tailwind CSS |
| Data fetching | React Query |
| Maps | Leaflet |
| Hosting | GitHub Pages |
| Data refresh | GitHub Actions + Python |
| PWA support | vite-plugin-pwa |

---

## Project structure

- `src/lib/prediction.ts` — the core prediction engine (pure functions, no side effects)
- `src/lib/config.ts` — crossing details and tuning constants
- `src/lib/api.ts` — fetches the live data
- `src/components/` — all UI pieces (status card, train list, timeline, map)
- `scripts/parse_cache.py` — parses the RailRadar API response into the app's data format
- `.github/workflows/refresh-data.yml` — fetches fresh train data every 5 minutes
- `.github/workflows/deploy.yml` — builds and publishes the site to GitHub Pages

---

## Running it locally

1. Clone the repo
2. Run `npm install`
3. Run `npm run dev`
4. Open the local URL shown in your terminal

The app will load using the seed data in `public/data/cache.json` until you connect a real API key.

---

## Credits

Built using live data from the [RailRadar API](https://railradar.in).

Want to set this up for a different railway crossing? See **DEPLOYMENT.md** for a step-by-step guide.
