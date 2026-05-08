# CarbonBridge Website

Official CarbonBridge platform by **Nomad Life Corporation** — a verified carbon‑credit marketplace.

This repository ships as a **single‑file static site**. Everything (UI, state, demo data, mock auth/orders via `localStorage`) lives in `index.html`, so it can be hosted on any static host with zero build step.

## Run it locally

Just open `index.html` in a browser, or serve the folder:

```bash
# Python 3
python3 -m http.server 8080

# or Node
npx serve .
```

Then visit http://localhost:8080.

## Deploy to Render

This repo includes a `render.yaml` blueprint that configures Render to serve the site as a Static Site with SPA fallback.

1. Push this repo to GitHub (already done).
2. In the [Render Dashboard](https://dashboard.render.com/), click **New → Blueprint** and select this repository. Render will read `render.yaml` and create the service automatically.
3. Click **Apply** — Render publishes the contents of the repo root.

That's it. There is no build step. Every push to `main` redeploys.

### What `render.yaml` does

- `runtime: static` — serves files directly, no Node/Python process.
- `staticPublishPath: .` — publishes the repository root (where `index.html` lives).
- `routes` — rewrites every path to `/index.html` so client‑side state survives a page refresh on any URL.
- `headers` — sensible security defaults (`X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`).

The `_redirects` file is included as a fallback for static hosts (Netlify, Cloudflare Pages, etc.) that consume that format instead of `render.yaml`.

## Project structure

```
.
├── index.html      # The entire website (React 18 via CDN, no build step)
├── render.yaml     # Render Blueprint: static site + SPA fallback
├── _redirects      # SPA fallback for Netlify-style hosts
├── .gitignore
├── LICENSE
└── README.md
```

## Notes for future work

- The site uses React 18 from `unpkg` and pure `React.createElement` (no JSX, no Babel runtime). Keep it that way for instant page loads, or migrate to a Vite build if you want JSX.
- "Auth", "wallet", and "orders" are mocked in `localStorage` for the demo. To go live, swap the `storage` object in `index.html` for real API calls.

© 2026 Nomad Life Corporation. All rights reserved.
