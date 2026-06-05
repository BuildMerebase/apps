# Wiki

A two-pane team wiki app. Pages are stored as markdown, rendered as formatted HTML. Left sidebar lists all pages alphabetically with live search. Click to read, click Edit (or Cmd+S) to write. First run auto-creates a Home page with a markdown cheatsheet.

**API:** Entity  
**Files:** `index.html`

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

Each wiki page is an entity of `type: "wiki_page"` with `name` (the page title) and `data: { body, updated }`. The entity list is filtered by `app_id` so pages are scoped to this deployment. The app includes an inline markdown parser that handles headings, bold, italic, strikethrough, inline code, fenced code blocks, unordered and ordered lists, blockquotes, links, and horizontal rules.

## Extension ideas

- Page history / revision log — store versions and allow rollback
- Table of contents — auto-generated from headings, floated in the margin
- Tags or categories — filter pages by tag in the sidebar
- Nested pages / parent-child hierarchy — tree view in the sidebar
- Backlinks — show which pages link to the current page
- Split view — side-by-side markdown source and rendered preview while editing
- Export to PDF or plain text
- Collaborative editing indicator — show who else is viewing a page

## Extend this app

Copy this prompt into Claude alongside the `index.html` source:

```
I have a merebase single-page app — a team wiki with markdown rendering.

merebase injects window.MB = { app_id, api } before </head>.
Each wiki page is a merebase entity (type: "wiki_page") with name and data: { body, updated }.
Entity API:
  GET  [api]/entities?app_id=[id]&type=[t]            - list entities
  POST [api]/entities {app_id, type, name, data, id?}  - save (create or update)
  POST [api]/entities/delete {id}                      - delete
All calls use credentials:'include'.

Here is the current source:
[paste index.html here]

I want to [describe what you want to add or change].

Keep it as a single index.html. Do not add a build step.
```
