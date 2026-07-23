# Ledger — Personal Finance Tracker

A single-file, installable web app for tracking cash expenses, income, and IOUs. Runs entirely in the browser — no backend, no account, no server. Your data lives in the device's local storage.

## Features

- **Dashboard** — live snapshot of current balance and recent activity
- **Add Entry** — log expenses, income, or IOUs
- **Spending** — category breakdown, charts (via Chart.js)
- **IOU Ledger** — track money owed to you and money you owe others, mark items settled
- **Monthly Report** — spending summary by month
- **Advisor** — optional AI chat about your finances (requires your own Gemini API key, entered in Settings — the key and your data never leave your browser except for that direct API call)
- **Excel import/export** — import a workbook with `Expenses`, `Income`, and `IOUs` sheets, or export your current data to `.xlsx`; a blank template is also downloadable from Settings
- **JSON backup/restore** — full state export/import for backups or moving devices

## Tech stack

- Plain HTML/CSS/JS, no build step
- [Chart.js](https://www.chartjs.org/) — charts
- [SheetJS (xlsx)](https://sheetjs.com/) — Excel import/export
- Browser `localStorage` — data persistence
- Service worker + Web App Manifest — installable, offline-capable PWA

## Project structure

```
ledger-pwa/
├── index.html      # the entire app (markup, styles, logic)
├── manifest.json    # app name, icons, standalone display mode
├── sw.js            # service worker — caches the app shell for offline use
├── icon-192.png      # home screen icon (192×192)
└── icon-512.png      # home screen icon (512×512)
```

## Running it locally

Any static file server works, since service workers require HTTP(S) (not `file://`):

```bash
cd ledger-pwa
python3 -m http.server 8000
```

Then open `http://localhost:8000` in a browser.

## Deploying

Host the folder anywhere that serves static files over HTTPS:

- **Netlify Drop** — drag the `ledger-pwa` folder onto [app.netlify.com/drop](https://app.netlify.com/drop) for an instant live URL
- **GitHub Pages** — push the folder to a repo, enable Pages on the `main` branch

## Installing on your phone

**iPhone (Safari):** open the deployed URL → Share → Add to Home Screen

**Android (Chrome):** open the deployed URL → ⋮ menu → Add to Home screen / Install app

Once installed, it opens full-screen like a native app and keeps working offline.

## Data & privacy

- All entries are stored in `localStorage` on the device — nothing is sent to a server.
- The only network calls the app makes are: loading Chart.js/xlsx from their CDNs on first load (then cached offline), and — only if you set an API key — your own requests to the Gemini API for the Advisor feature.
- Resetting the app (Settings → Reset) clears local data and restores the originally imported Excel data.

## Excel template format

| Sheet | Columns |
|---|---|
| Settings | Field, Value, Notes |
| Expenses | Date, Category, Description, People (comma separated), Amount |
| Income | Date, Source, Note, Amount |
| IOUs | Date, Direction, Person, Amount, Reason, Settled, Type |

Download a blank copy anytime from Settings → Download Template.
