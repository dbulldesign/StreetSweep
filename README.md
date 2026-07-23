# 🧹 StreetSweep

An interactive map of NYC street cleaning (Alternate Side Parking) schedules and parking rules. Open it on your phone or laptop, opt in to share your location, and instantly see when cleaning happens on your block, which days ASP is suspended, and the full parking rules.

**Live app:** https://dbulldesign.github.io/StreetSweep/ *(after you enable GitHub Pages — see below)*

---

## Features

- **Interactive dark map** of all five boroughs (Leaflet + CartoDB dark tiles). Tap any street to select that point and open a detail panel for the nearby block.
- **"Find me" geolocation** — opt-in only. Nothing runs until you tap 📍. Shows your position + accuracy circle, recenters, and auto-loads the nearby schedule. Permission denial is handled with a friendly message, not a crash.
- **Parking-sign layer** (toggle the sign button, bottom-right) — plots real NYC parking signs at their true curbside locations. Street-cleaning (ASP) signs are highlighted with a broom symbol; all other regulation signs show as blue markers. Tap any sign for its full regulation text and side of street. Signs load for whatever area you're viewing (zoom in to see them) and collapse into numbered clusters when you zoom out; tap a cluster to zoom into that block.
- **Draggable bottom sheet** — drag the handle up/down to expand or collapse the detail panel; it snaps to the nearest position. Tap it to toggle.
- **Today banner** — computes whether ASP today is suspended (planned holiday), a normal cleaning day, or Sunday (no ASP), plus a countdown to the next planned suspension.
- **2026 suspension calendar** (📅) — searchable modal listing every planned citywide ASP suspension. Past dates are dimmed; today is badged.
- **Per-block detail card** — Sun–Sat day chips highlighting cleaning days, hours, side of street, a today-status badge, and the standard NYC parking rules.
- **Nearby parking-meter rates** — when a meter blockface is close to the tapped point, the panel adds a card with the rate, hours in effect, and ParkNYC zone number. Omitted gracefully when there's no meter nearby.
- **Live-cancellation links** — direct links to NYC311 and Notify NYC for same-day weather/emergency suspensions (see limitations below).
- **Single file, no build step, no dependencies to install.** Just one `index.html`.

---

## Data sources

| Data | Source |
|------|--------|
| Per-block cleaning schedule | [NYC Open Data (DSNY) — Socrata API](https://data.cityofnewyork.us/) |
| Parking signs (all regulations) | [NYC DOT — Parking Regulation Locations and Signs (`nfid-uabd`)](https://data.cityofnewyork.us/Transportation/Parking-Regulation-Locations-and-Signs/nfid-uabd) |
| Parking-meter rates | NYC DOT — Parking Meter rates / ParkNYC blockfaces (`e7yp-wx55`) |
| 2026 planned suspension calendar | NYC Department of Sanitation (DSNY) |
| Base map tiles | CartoDB (dark) + OpenStreetMap |
| Map rendering | [Leaflet](https://leafletjs.com/) + [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster) (both via CDN — the only external dependencies) |

> **Note on sign positions:** NYC publishes sign locations in NY State Plane coordinates (EPSG:2263, feet), not latitude/longitude. StreetSweep converts them to map coordinates in-browser (a self-contained Lambert Conformal Conic transform — no library), so signs land on the correct side of the street. Only *current* signs are shown (the dataset also carries historical/removed ones). Like the per-block schedule, this layer needs HTTPS hosting to reach the live feed.
>
> **Optional:** the map queries NYC Open Data on each pan. To lift the anonymous 1,000-requests/hour throttle, register a free [Socrata app token](https://data.cityofnewyork.us/profile/app_tokens) and paste it into the `SOCRATA_APP_TOKEN` constant near the top of the sign-layer script. It's read-only and safe to ship in the page.

---

## Known limitations (read before relying on it)

1. **The physical sign on the curb is always the legal source of truth.** Schedules vary block to block and even by side of street; the open dataset does not map every individual sign.
2. **Live same-day cancellations (snow/emergencies) are a link, not automation.** No reliable public API exists for weather-driven suspensions — the city posts the official daily status just after midnight. The app deliberately does **not** fabricate a same-day status. On a suspension-prone day, always confirm via the in-app NYC311 / Notify NYC links.
3. **Live per-block schedule lookups require HTTPS hosting.** Opening the file directly from your computer (`file://`) will gracefully fall back to showing the parking rules + calendar, because browsers block cross-origin API calls from local files. GitHub Pages (below) serves it over HTTPS, which fixes this.

---

## Setup — publish it live with GitHub Pages

1. Add `index.html` to this repo (it's already here).
2. Go to **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Set branch to **main** and folder to **/ (root)**, then **Save**.
5. Wait ~1 minute. Your app will be live at:
   `https://dbulldesign.github.io/StreetSweep/`

That's it — bookmark that URL on your phone's home screen and it behaves like an app.

---

## Local use

You can also just open `index.html` in any browser. The map, geolocation, parking rules, and the suspension calendar all work offline; only the live per-block schedule API needs the HTTPS hosting above.

---

## Privacy

No tracking, no analytics, no external fonts. Your location never leaves your device — it's used only to center the map and query the public NYC dataset from your browser.

---

## Version

The current version shows as a badge next to the "StreetSweep" title in the header, in the "Today" panel footer, and in the browser console on load. Bump `APP_VERSION` / `APP_BUILT` near the top of the script in `index.html` on each change.

| Version | Notes |
|---|---|
| 1.6.1 | Fix sheet drag: lock page scroll (no rubber-band) and make the whole grip+header draggable |
| 1.6.0 | Remove on-screen zoom buttons (pinch to zoom); compass now alone in top-right |
| 1.5.0 | Fix bottom-sheet clipping on mobile/PWA (scroll + safe-area); compass docked as a map control |
| 1.4.0 | Compass indicator (north-up orientation) |
| 1.3.0 | Version badges; finalized meter-rates card against real schema |
| 1.2.0 | Nearby parking-meter rates card (e7yp-wx55) |
| 1.1.0 | Parking-sign layer: all signs, ASP highlighted, clustered (nfid-uabd) |
| 1.0.0 | Map, geolocation, draggable sheet, today banner, calendar, per-block rules |

---

## License

MIT — free to use, modify, and share.

---

*Not affiliated with the City of New York or DSNY. Always follow posted signage.*
