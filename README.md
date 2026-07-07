# Flory Freedom

A single-file retirement / financial-independence planning PWA.
Live app: **https://florymj.github.io/flory-freedom/** (GitHub Pages).

## What it does

Models retirement readiness with Monte Carlo simulation, guardrails
spending, bridge phases before TRS/Social Security, ACA/MAGI planning,
budget layers, and portfolio import from Fidelity CSVs. Installable as a
PWA (offline via service worker).

## Architecture

- **`index.html`** — the entire app (~2,500 lines): React 18 via CDN
  (`React.createElement`, no JSX), inline CSS, all state and simulation
  logic. There is intentionally **no build system, bundler, or
  node_modules** — do not add one.
- **`sw.js`** — service-worker cache of the app shell. Bump `CACHE_NAME`
  when shipping changes so clients pick up the new version.
- **`manifest.json`**, icons, `.nojekyll` — PWA install + Pages plumbing.
- **`CODEX.md`** — running task brief for AI coding agents; P0 and P1
  redesign tasks are complete. Read it before making changes.
- **`private/fidelity-imports/`** — local CSV drop folder (contents not
  for the app at runtime).

## Development

No install step. Open `index.html` in a browser, or serve the folder
(`python -m http.server`) for service-worker testing. State persists in
`localStorage` under `flory-freedom-v1`; `normalizeState` backfills new
defaults automatically.

## Deploying

Push to `main` — GitHub Pages serves the repo root. Remember to bump
`CACHE_NAME` in `sw.js` or returning visitors will keep the old cached
build.

## Conventions

- Keep it single-file; no JSX, no bundler.
- Don't change the color palette variables or dark theme.
- See issue #1 (Project handoff checklist) for the handoff log.
