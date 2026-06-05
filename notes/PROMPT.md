# Notes

A two-pane notes app. List on the left, editor on the right. Notes save automatically as you type. Each note is a separate entity record with a title and body.

**API:** Entity  
**Files:** `index.html`

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

Each note is an entity of `type: "note"` with `name` (the title) and `data: { body, updated }`. The entity list is filtered by `app_id` so notes are scoped to this deployment. Auto-save debounces 800ms after the last keystroke.

## Extension ideas

- Tags or folders for organising notes
- Full-text search across note bodies
- Markdown rendering in a read/edit toggle
- Pin important notes to the top
- Share a note by generating a public link
- Note templates for common formats (meeting notes, decisions, etc.)

## Extend this app

Copy this prompt into Claude alongside the `index.html` source:

```
I have a merebase single-page app - a two-pane notes app.

merebase injects window.MB = { app_id, api } before </head>.
Each note is a merebase entity (type: "note") with name and data: { body, updated }.
Entity API:
  GET  [api]/entities?app_id=[id]&type=[t]           - list entities
  POST [api]/entities {app_id, type, name, data, id?} - save (create or update)
  POST [api]/entities/delete {id}                     - delete
All calls use credentials:'include'.

Here is the current source:
[paste index.html here]

I want to [describe what you want to add or change].

Keep it as a single index.html. Do not add a build step.
```
