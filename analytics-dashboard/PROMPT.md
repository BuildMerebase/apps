# Analytics Dashboard

A dark-themed executive analytics dashboard with 6 KPI cards, a 30-day revenue and orders trend chart, traffic source donut, category bar chart, and a live activity feed. All chart rendering is pure canvas — no external libraries. Data is stored in the KV API under a single `dashboard` key.

## Structure

Single `index.html` — fully self-contained with inline CSS and JS.

## Data shape

All dashboard data lives in one KV key: `dashboard`

```json
{
  "metrics": {
    "revenue":    { "label": "Revenue",      "value": 284750, "prev": 253600, "fmt": "$", "suffix": "",  "sparkline": [210,...], "color": "var(--blue)" },
    "users":      { "label": "Active users", "value": 18420,  "prev": 16940,  "fmt": "",  "suffix": "",  "sparkline": [...],      "color": "var(--green)" },
    "orders":     { "label": "Orders",       "value": 3847,   "prev": 3926,   "fmt": "",  "suffix": "",  "sparkline": [...],      "color": "var(--amber)" },
    "conversion": { "label": "Conversion",   "value": 3.8,    "prev": 3.4,    "fmt": "",  "suffix": "%", "sparkline": [...],      "color": "var(--cyan)" },
    "avg_order":  { "label": "Avg order",    "value": 74.02,  "prev": 70.35,  "fmt": "$", "suffix": "",  "sparkline": [...],      "color": "var(--purple)" },
    "sessions":   { "label": "Sessions",     "value": 485300, "prev": 421000, "fmt": "",  "suffix": "",  "sparkline": [...],      "color": "var(--red)" }
  },
  "trend": {
    "labels":  ["May 5", "", ...],
    "revenue": [8200, 7800, ...],
    "orders":  [108, 102, ...]
  },
  "sources": [
    { "name": "Organic search", "value": 42, "color": "#3B82F6" }
  ],
  "categories": [
    { "name": "Software", "current": 89400, "prev": 78200 }
  ],
  "activity": [
    { "time": "2m ago", "event": "Enterprise signup", "detail": "Acme Corp", "type": "success" }
  ]
}
```

## Extending this app

Tell Claude:

- "Add a new KPI card for [metric name]" — add a key to `metrics` following the same shape
- "Change the colour scheme to light mode" — swap the CSS custom properties in `:root`
- "Add a data entry form so I can update the metrics" — Claude will add an edit panel that calls `DB.set('dashboard', data)`
- "Add a world map showing revenue by region" — Claude can draw SVG paths for a simplified map
- "Replace the activity feed with a top customers table" — swap the bottom-right panel
- "Make the charts update every 30 seconds" — wrap `init()` in `setInterval`
- "Add a date range filter" — slice the trend arrays and re-render

## Notes

- Click "Load sample data" on first deploy to populate with realistic demo data
- The dashboard is read-only by default — add an admin page for editing
- All chart drawing uses HTML5 Canvas — no dependencies, works everywhere
- Built for inline deployment: CSS and JS are embedded in the HTML file
