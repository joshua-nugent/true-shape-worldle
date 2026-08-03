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

---

## 👋 For Tony — how to make changes yourself

Hi Tony! This is the whole game. There are two ways to edit it — start with **Option A**, it needs
nothing installed. Use **Option B** later if you want to actually run and test the game on your own
computer before publishing.

### First-time setup (5 minutes, do this once)

1. Make a free account at **[github.com](https://github.com/signup)**.
2. Tell Josh your username — he'll give you access to this project.
3. You'll then be able to open this project at
   **https://github.com/joshua-nugent/true-shape-worldle** and edit anything in it.

### Option A — Edit right in the website (easiest, nothing to install)

This is the simplest way to make a small change like fixing text or changing a color.

1. Go to the project page and click the file you want to change. **Almost everything is in one file:
   `index.html`** — that's the game's appearance, text, and rules all together.
2. Click the **pencil ✏️ icon** (top-right of the file) to edit it.
3. Make your change. (See "What to change for common tweaks" below.)
4. Scroll to the bottom, type a short note in the **"Commit changes"** box describing what you did
   (e.g. *"made the title bigger"*), and click the green **Commit changes** button.
5. That's it — your change is saved. If the site is connected to Railway (see the Deploy section),
   the live website updates by itself a minute later.

> 💡 Made a mistake? Nothing you do here can break anything permanently — every change is saved in
> history and can be undone. Ask Josh to "revert" and it goes back instantly.

### Option B — Run it on your own computer (to test before publishing)

Do this when you want to *see* your changes working before they go live.

**One-time installs:**

1. Install **[GitHub Desktop](https://desktop.github.com/)** — this copies the project to your
   computer and handles saving/publishing with buttons instead of typing commands.
2. Install **[Node.js](https://nodejs.org/)** — click the big "LTS" button and run the installer.
   (This is what runs the game on your computer.)

**Each time you want to work on it:**

1. Open **GitHub Desktop** → *File → Clone repository* → pick `true-shape-worldle`. This downloads
   a copy to your computer (only needed the first time; after that it's already there).
2. Open the project folder. On a **Mac**, open the **Terminal** app; on **Windows**, open
   **Command Prompt**. Then type this and press Enter:
   ```
   npm start
   ```
3. Open your web browser and go to **http://localhost:3000** — you're now playing your local copy.
4. Edit `index.html` in any text editor (even TextEdit works, or the free
   [VS Code](https://code.visualstudio.com/)). Save the file, then **refresh the browser** to see
   your change.
5. When you're happy: open **GitHub Desktop**, type a short summary at the bottom-left, click
   **Commit to main**, then click **Push origin** at the top. Your change is now published.

> ⚠️ Important: you must use `npm start` and open `http://localhost:3000`. If you just
> double-click `index.html` to open it, the map won't load (browsers block the country data from
> loading that way). This is normal and expected.

### What to change for common tweaks

Everything below lives in **`index.html`**. Use your editor's Find (Ctrl-F / Cmd-F) to jump to it:

| You want to… | Find this | Change |
|---|---|---|
| Change the title or tagline | `True Shape Worldle` | Edit the text |
| Give more/fewer guesses | `MAX_GUESSES = 6` | Change the `6` |
| Change the colors | `--bg:` `--accent:` `--land:` (near the top) | Edit the color codes |
| Change the projection labels | `True shape`, `Mercator`, `Gall–Peters` | Edit the text |
| Reword the "How to play" help | search for `How to play` | Edit the paragraphs |

If you want a bigger change (new features, new game modes), just describe it to Josh — that's the
kind of thing worth having Claude Code help with.

---

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
