# Chore Champions

A single-file HTML/CSS/JS chore tracker for two kids, Charlie and Bertie. No build system — the entire app is `chore-tracker.html`.

## Architecture

**Frontend:** Pure HTML/CSS/JS, no frameworks, no bundler. Edit `chore-tracker.html` directly.

**Backend:** Google Apps Script (external). The frontend fetches from a URL stored in `localStorage` under `choreChampions_scriptUrl`. The app works fully offline with default chores if no URL is configured.

**Data flow:**
- `loadChores()` — `GET ?action=getChores` → `{ chores: [...], date: "..." }`
- `saveChore()` — `GET ?action=updateChore&id=...&doneBy=...`
- `resetDay()` — `GET ?action=resetDay`

## Kids & Colours

| Kid     | CSS var       | Hex       |
|---------|---------------|-----------|
| Charlie | `--charlie`   | `#FF6B6B` |
| Bertie  | `--bertie`    | `#4ECDC4` |

Kid names are used as string keys throughout (`"Charlie"`, `"Bertie"`). CSS classes are the lowercase versions (`charlie`, `bertie`).

## Key Conventions

- **Never use `innerHTML` with server-supplied data** — chore names/emojis come from Google Sheets. Use `textContent` or DOM methods.
- **Always check `res.ok`** before calling `res.json()` on fetch responses.
- **Confetti** uses the Web Animations API (`element.animate()`), not injected `<style>` tags.
- **Loading overlay** is shown/hidden via the `.show` CSS class, not inline `display` style.
- **Tab switching** uses `data-view` attributes on `.view-tab` buttons, not DOM index.
- **Sync guard** — `syncingChores` is a `Set` of chore IDs currently being saved. Check before toggling to prevent double-saves.

## Default Chores

Defined in `DEFAULT_CHORES` array. Each chore: `{ id, emoji, name }`. The `doneBy` field (`"Charlie"`, `"Bertie"`, or `null`) is added at runtime.

## No Tests, No CI

Manual testing only. Open `chore-tracker.html` directly in a browser.
