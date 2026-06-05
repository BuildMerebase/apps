# HUD Dashboard

A Jarvis/Iron Man style ambient HUD dashboard with a rotating radar, animated PCB circuit traces, live clock, and 6 real-time metric panels. All rendering is pure canvas and inline HTML — no external libraries or dependencies.

## Structure

Single `index.html` — fully self-contained with inline CSS and JS.

## Data shape

All dashboard data lives in one KV key: `hud`

```json
{
  "system": "MEREBASE.OS",
  "metrics": [
    { "id": "m1", "label": "ACTIVE USERS",  "value": "1,247", "unit": "",    "trend": "+12%",  "status": "normal" },
    { "id": "m2", "label": "API REQUESTS",  "value": "48.2K", "unit": "/hr", "trend": "+5%",   "status": "normal" },
    { "id": "m3", "label": "STORAGE",       "value": "2.4",   "unit": "GB",  "trend": "+0.1",  "status": "normal" },
    { "id": "m4", "label": "RESPONSE TIME", "value": "142",   "unit": "ms",  "trend": "-8ms",  "status": "good"   },
    { "id": "m5", "label": "APPS DEPLOYED", "value": "23",    "unit": "",    "trend": "+2",    "status": "normal" },
    { "id": "m6", "label": "ERROR RATE",    "value": "0.02",  "unit": "%",   "trend": "-0.01", "status": "good"   }
  ],
  "alerts": []
}
```

Status values: `normal` (cyan), `good` (green), `warning` (amber), `critical` (red).

## Editing metrics

Triple-click anywhere on the top bar to open the admin panel. You can edit all metric labels, values, units, trends, and statuses, as well as the system name. Click **SAVE & DEPLOY** to persist changes to the KV store.

## Extending this app

Tell Claude:

- "Change the system name to ACME CORP" — update `system` in the data model
- "Add a critical alert for high memory usage" — push a string to the `alerts` array in the KV data
- "Add a 7th metric for CPU usage" — add a new object to the `metrics` array following the same shape, then update the grid CSS to `repeat(4, 1fr)` rows
- "Set the error rate metric to warning status" — change `status` to `"warning"` in that metric's object
- "Change the radar sweep speed" — adjust the `10000` constant in `(2 * Math.PI / 10000) * dt`
- "Add a second metrics page that cycles on a timer" — Claude can add a setInterval to swap between two datasets
- "Make the background traces more visible" — increase the opacity values in `rgba(0,212,255,0.06)` and `rgba(0,212,255,0.4)` in the drawBg function

## Notes

- On first deploy with no stored data the app seeds itself from the built-in defaults and saves to KV automatically
- Open locally as a plain HTML file for a full animated preview with mock data — no server needed
- The boot sequence animation runs on every page load
- Triple-click the top bar to access the admin panel for live data editing
