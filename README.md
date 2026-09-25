# Arco Café

The website for **Arco Café** — "Where Every Cup Tells a Story." A static, restaurant-style marketing site with a live menu, online ordering, table reservations, and a photo gallery, all backed by a Google Apps Script + Google Sheets backend.

## Pages

| Page | File | Description |
| --- | --- | --- |
| Home | `index.html` | Hero, featured/special menu items, reviews, café settings/hours |
| Menu | `menu.html` | Full menu, pulled live from the backend |
| Order Online | `order.html` | Browse the menu and place an order |
| Reserve a Table | `reservation.html` | Table reservation form, reads live settings (e.g. hours/capacity) |
| Gallery | `gallery.html` | Photo gallery, pulled live from the backend |

## Tech stack

| Layer     | Technology                                          |
| --------- | ---------------------------------------------------- |
| Frontend  | Plain HTML, CSS, JavaScript — no framework, no build step |
| Backend   | Google Apps Script (Web App)                          |
| Database  | Google Sheets                                          |
| Fonts     | Cormorant Garamond, DM Sans (Google Fonts)              |

All dynamic content — menu items, specials, reviews, gallery photos, café settings, orders, and reservations — is read from and written to a single Google Apps Script Web App, configured in `app.js`.

## Project structure

```
index.html          # Home page
menu.html             # Menu page
order.html             # Online ordering page
reservation.html        # Table reservation page
gallery.html              # Photo gallery page
app.js                     # Shared config — GAS_URL (single source of truth
                            # for the Apps Script Web App endpoint)
style.css                   # All site styles
```

## Setup

### 1. Set up the Google Apps Script backend

The frontend expects a single Apps Script Web App that responds to `GET` requests with an `action` query param and `POST` requests with an `action` field in the JSON body:

| Action | Method | Purpose |
| --- | --- | --- |
| `menu` | GET | Fetch menu items (supports `limit` and `special` query params, used on the home page for featured items) |
| `gallery` | GET | Fetch gallery photos |
| `reviews` | GET | Fetch customer reviews |
| `settings` | GET | Fetch café settings (hours, etc.) |
| `order` | POST | Submit a new order |
| `reservation` | POST | Submit a new table reservation |

Deploy your Apps Script project (bound to a Google Sheet holding menu, gallery, reviews, settings, orders, and reservations data) as a **Web App** with *Execute as* `Me` and *Who has access* `Anyone`, then copy the deployed URL (ends in `/exec`).

### 2. Configure the frontend

Edit `app.js`:

```js
const GAS_URL = 'YOUR_DEPLOYED_APPS_SCRIPT_URL'; // ends in /exec
```

Every page loads `app.js` before its own script and falls back gracefully (no live data, no errors) if `GAS_URL` is left as the placeholder.

### 3. Run locally

No build step is required — open `index.html` directly in a browser, or serve the folder with any static file server:

```bash
npx serve .
```

### 4. Deploy

Host the folder on any static host (GitHub Pages, Netlify, Vercel, etc.). The Apps Script Web App is already publicly reachable once deployed, so no backend hosting is needed.

## Notes

- The "Reserve a Table" links on the nav point to `#reserve` on the home page; the dedicated reservation flow lives at `reservation.html`.
- Order and reservation submissions depend entirely on `GAS_URL` being configured — until then, `order.html` and `reservation.html` will not persist submissions anywhere.
