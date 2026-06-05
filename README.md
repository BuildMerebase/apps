# merebase starter apps

Ready-to-deploy apps for [merebase](https://merebase.com) - the WordPress plugin that turns your site into an app platform. Each app is a `.zip` file. Upload it via the merebase dashboard and it is live in seconds, with persistent storage, authentication, and access control included.

These are working apps, not wireframes. Deploy one, use it with your team, and extend it. Each app folder contains a `PROMPT.md` with a ready-to-use Claude prompt for extending that specific app.

**Running locally:** You can open any app directly in a browser to preview the design and layout. Without merebase as a backend, each page runs on sample data in memory - nothing is saved and nothing persists between refreshes or page navigations. To see the full functionality - persistent storage, shared data across users, and multi-page data flow - deploy the zip to a merebase-enabled WordPress site.

---

## The apps

### To-do list
`dist/todo.zip` - KV - Single page

A shared to-do list. Anyone with access sees the same list. Add tasks, check them off, filter by status. All tasks are stored as a JSON array under a single KV key.

Extension ideas: per-user lists, due dates, priority levels, categories, assign to team members, activity log.

---

### Habit tracker
`dist/habit-tracker.zip` - KV - Single page

Track daily habits with a streak counter and weekly grid view. Each habit stores a record of which dates it was completed. Streak is calculated from the completion history.

Extension ideas: per-user tracking, weekly/monthly charts, habit categories, custom streak goals, notes per completion.

---

### Notes
`dist/notes.zip` - Entity - Single page

A two-pane notes app. List on the left, editor on the right. Notes save automatically as you type. Each note is a merebase entity record with a title and body, scoped to this app.

Extension ideas: tags and folders, full-text search, markdown rendering, pinned notes, shareable note links, note templates.

---

### Project tracker
`dist/project-tracker.zip` - Entity + Relations - Single page

Projects contain tasks. Each project and each task is a separate entity. A relation of type `has_task` links them. Deleting a project cascades - removes all linked task entities and their relations.

Extension ideas: due dates, task priority, assignees, comments per task, milestones, completion burndown, export.

---

### Client manager
`dist/client-manager.zip` - Entity + Relations - Single page

A three-level CRM: clients contain projects, projects contain interactions. Each level is a separate entity type linked by relations. Interaction types: note, call, email, meeting.

Extension ideas: pipeline stages, deal values, document links, bulk import, dashboard stats, CSV export.

---

### Team noticeboard
`dist/noticeboard.zip` - KV - Multi-page zip

An internal announcement board with multiple pages and a shared stylesheet. Admin posts announcements. Each post has its own shareable URL. The board can go on a screen, in a Slack channel, or be bookmarked by the team.

Pages:
- `index.html` - the board (share with your whole team)
- `post.html?id=...` - single post with copy-link button
- `admin.html` - create posts, pin, delete, configure board name

Extension ideas: comments on posts, emoji reactions, scheduled posts, department-specific boards, read receipts, archive after N days.

---

### AI tool registry
`dist/ai-registry.zip` - KV - Multi-page zip

An organisational AI adoption register with status tabs, category filters, and live search. Each tool has a contact person, enablement link, and next-review date with visual overdue alerts. Ships pre-loaded with 20 real tools across 9 categories.

Pages:
- `index.html` - registry board
- `tool.html?id=...` - tool detail with shareable URL
- `admin.html` - add, edit, and manage tools

Extension ideas: approval workflow, usage tracking, licence cost tracking, user ratings, comparison view, Slack webhook on status change.

---

### Analytics dashboard
`dist/analytics-dashboard.zip` - KV - Single page

A dark executive analytics dashboard with 6 KPI metric cards (each with a sparkline), a 30-day dual-axis revenue and orders trend chart, a traffic sources donut, a category revenue bar chart, and a live activity feed. All charts are drawn with HTML5 Canvas — no external libraries. Click "Load sample data" on first deploy to populate with realistic demo data.

Data is stored in a single KV key `dashboard` as a structured JSON object. To update the numbers, write new values to that key from any app or script.

Extension ideas: editable metrics panel, date range filter, auto-refresh every N seconds, alert thresholds with colour changes, export to CSV, additional chart types, light mode toggle.

---

### Wiki
`dist/wiki.zip` - Entity - Single page

An internal team wiki for dumping and growing markdown pages. Left sidebar lists all pages alphabetically with live search. Click any page to read it — markdown renders as HTML with headings, lists, code blocks, blockquotes, and links. Click Edit to write or update in a monospace textarea and save. Pages are individual entity records so they can grow independently.

Extension ideas: page linking (`[[Page name]]` syntax), version history, tags and categories, full-text search, page templates, read receipts, public/private per-page visibility.

---

### Pitch deck
`dist/pitch-deck.zip` - KV - Single page

A fullscreen startup pitch deck builder and presenter. Three slide templates — title (centered headline, tagline, byline), content (headline, intro, bullet points), and stats (headline, big number/label blocks). Click directly on any text to edit it in place. Hover a block to reveal a `×` to delete it; clearing text and clicking away removes it automatically. Add bullets or stat blocks individually within a slide.

The entire deck is stored as one JSON object under the KV key `deck`. Ships with a 6-slide Series A sample deck. Toggle edit mode with the Edit button; press **F** for fullscreen; arrow keys or spacebar to navigate slides.

Extension ideas: slide background images, PDF export via print stylesheet, presenter view in a second window, a read-only share link, a countdown timer overlay, per-slide transition styles.

---

### HUD dashboard
`dist/hud-dashboard.zip` - KV - Single page

A Jarvis-style ambient HUD display. Pulsing orb with speech-pattern amplitude bursts, animated equaliser bars, radar sweep, and a hacker-terminal log — all in the centre column. Six metric tiles per side, each with a live sparkline, red-to-green gradient bar, and secondary figures. Everything driven by Canvas and requestAnimationFrame with no external libraries.

The full dashboard state is stored under the KV key `hud` as one JSON object. Triple-click the top bar to open admin mode and edit any metric label, value, unit, trend, or status. Ships with sample data — click deploy and it's live immediately.

Extension ideas: auto-refresh metrics from an external API, alert thresholds that turn tiles red, additional tile types (arc gauge, status list), fullscreen kiosk mode, per-metric historical data for longer sparklines.

---

## How deployment works

1. Log in to your WordPress site
2. Go to `/mb/dashboard/`
3. Click **+ Upload app**
4. Upload the zip - merebase extracts it, assigns an `app_id`, and serves it at `/mb/app/your-slug/`

The first deploy creates the app. Future deploys to the same `app_id` update it without touching stored data.

### What merebase injects

Before serving any HTML file, merebase injects `window.MB` into `<head>`:

```js
window.MB = {
  version: "1.0.x",
  app_id:  "uuid",
  slug:    "your-slug",
  status:  "online",
  api:     "https://yoursite.com/mb/api"
}
```

Every API call uses `MB.app_id` to scope data to this app and `credentials: 'include'` to send the session cookie. No API key needed inside the app itself.

---

## The two storage models

### KV - key/value storage

Simple, fast, per-app. Good for settings, lists, and structured objects that don't need querying.

```js
const r = await fetch(`${MB.api}/data?app_id=${MB.app_id}&key=mykey`, { credentials:'include' });
const { value } = await r.json();

await fetch(`${MB.api}/data`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  credentials: 'include',
  body: JSON.stringify({ app_id: MB.app_id, key: 'mykey', value: { anything: true } })
});
```

Used by: todo, habit-tracker, noticeboard, ai-registry, analytics-dashboard, pitch-deck

### Entity + Relations - structured records

Named records with typed data and optional relationships between them. Good for anything with a real data model.

```js
const r = await fetch(`${MB.api}/entities?app_id=${MB.app_id}&type=note`, { credentials:'include' });
const { entities } = await r.json();

await fetch(`${MB.api}/entities`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  credentials: 'include',
  body: JSON.stringify({ app_id: MB.app_id, type: 'note', name: 'Title', data: { body: '...' } })
});
```

Used by: notes, project-tracker, client-manager

---

## Extending an app

Each app folder has a `PROMPT.md` with a pre-filled Claude prompt for that specific app. Open the folder, read the prompt, paste it into Claude alongside the source, and describe what you want to add.

---

## Repo structure

```
apps/
  dist/               - ready-to-upload zips
  todo/               - source + PROMPT.md
  habit-tracker/      - source + PROMPT.md
  notes/              - source + PROMPT.md
  project-tracker/    - source + PROMPT.md
  client-manager/     - source + PROMPT.md
  noticeboard/           - source + PROMPT.md
  ai-registry/           - source + PROMPT.md
  analytics-dashboard/   - source + PROMPT.md
  wiki/                  - source + PROMPT.md
  pitch-deck/            - source + PROMPT.md
  README.md              - this file
```
