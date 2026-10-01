# Chhouk Va II — Village Map & Directions

A single-page, static web app that shows **Borey New World Chhouk Va II** (បុរីពិភពថ្មីឈូកវ៉ា២) as a clean custom map and gives **turn-by-turn walking directions** from the user's live GPS position to predefined places.

No backend, no map tiles, no API keys — everything runs in the browser.

## Files
| File | Purpose |
|------|---------|
| `index.html` | The whole app (HTML + CSS + JS). |
| `map-data.json` | The village data (roads, blocks, street names) baked from OpenStreetMap, clipped to the borey. |
| `map-data-full.json` | Backup: the wider area before clipping. |
| `osm-raw.json` | Raw OpenStreetMap download (for regenerating). |
| `preview.png` / `preview.svg` | Static preview image. |

## Features
- Custom **SVG map** of the exact block shapes + street names (Khmer/English).
- Map is **rotated** so streets align to the screen's X/Y axes, centered and full-screen.
- Panning is **locked** at default zoom and **constrained** to the borey when zoomed in.
- **Live GPS dot** with accuracy circle (needs HTTPS — see below).
- **Red pins for predefined places only**; tap a pin for turn-by-turn directions (in-browser Dijkstra over the real street network).

## Adding places
Edit the `PLACES` array near the top of the `<script>` in `index.html`:
```js
const PLACES=[
  {name:"សាច់គោងៀត", lng:104.81659372797475, lat:11.57151299561483},
  // {name:"...", lng:..., lat:...},
];
```
`lng` (longitude) first, then `lat` (latitude). Get coordinates by right-clicking a spot in Google Maps.

## Running locally
Geolocation needs a secure context, so serve it (don't double-click the file):
```bash
python3 -m http.server 8000
# then open http://localhost:8000/index.html
```
`localhost` counts as secure, so GPS works locally.

## Publishing (so phones get GPS)
GPS requires **HTTPS**. Easiest free option is GitHub Pages:
1. Push this folder to a GitHub repo.
2. Repo → Settings → Pages → deploy from `main` branch, root.
3. Open the `https://<user>.github.io/<repo>/` link on your phone.

## Regenerating the map data
Data comes from OpenStreetMap via the Overpass API, then clipped to the borey boundary and rotated/labelled at runtime. Note: **Street 3** was missing a name in OSM and is added manually in `map-data.json`.

Data © OpenStreetMap contributors (ODbL).
