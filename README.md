# 🌐 True Shape Worldle

A [Worldle](https://worldle.teuteuf.fr/)-style country-guessing game with a cartographic twist:
countries are shown in their **true shape** (an orthographic view, as if seen from directly
overhead in space) instead of the distorted Mercator silhouettes the original uses — and you can
**toggle the same country live** between three projections to see how each one lies.

| Projection | Preserves | Distorts |
|---|---|---|
| **Orthographic** (default, "true shape") | shape & area near the center | far edges of the globe |
| **Mercator** | local shape | area (huge toward the poles) |
| **Gall–Peters** | area | shape (stretched) |

No flat map keeps both shape and area true — only a globe can. That trade-off is the whole point.

## Play

- 6 guesses per country. After each guess: **distance**, a **direction arrow**, and **proximity %**.
- A new daily puzzle, plus a **Practice** button for unlimited rounds.
- Results (streaks, guess distribution, history) are stored **locally on your device** — no login, no accounts.

## Run locally

```bash
npm start        # serves on http://localhost:3000 (or $PORT)
```

Then open http://localhost:3000. Serve it — don't open `index.html` via `file://`, because the
browser blocks the `fetch()` of the local country data under the `file:` protocol.

## Deploy to Railway

The repo is deploy-ready with zero config:

1. Push to GitHub (done).
2. In Railway: **New Project → Deploy from GitHub repo** → pick this repo.
3. Railway (via Nixpacks) detects Node from `package.json`, runs `npm start`, and injects `$PORT`.

`railway.json` pins the builder and start command explicitly. No build step, no dependencies to install.

## Stack

- Plain HTML/CSS/JS — no framework, no bundler.
- [D3](https://d3js.org/) + [d3-geo-projection](https://github.com/d3/d3-geo-projection) for the map math (vendored in `vendor/`, so it runs offline).
- Country outlines with names from [johan/world.geo.json](https://github.com/johan/world.geo.json) (`data/countries.geo.json`).
- A ~40-line zero-dependency Node static server (`server.js`).

## Notes & known limitations

- **Antarctica** is never the answer (it can't be drawn in Mercator — it sits on the pole).
- Country outlines are simplified (110m resolution). Tiny island nations may be hard to recognize.
- The projection difference is dramatic for large / high-latitude countries (Greenland, Russia,
  Canada) and subtle for small equatorial ones — that's inherent to how projections work, not a bug.
