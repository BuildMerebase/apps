# AI tool registry

An organisational AI adoption register. Track which tools are approved, under evaluation, or rejected. Each tool has a contact person, enablement link, and next-review date with visual overdue alerts. Ships pre-loaded with 20 real tools across 9 categories.

**API:** KV  
**Files:** `index.html`, `tool.html`, `admin.html`, `style.css`, `data.js`

## Pages

- `index.html` - registry board. Status tabs (All / Approved / Experimental / Rejected) with counts, category filter chips, live search, colour-coded tool cards.
- `tool.html?id=...` - tool detail. Full description, status badge, contact, enablement link, review date chip (green/amber/red by urgency). Shareable URL per tool.
- `admin.html` - add and edit tools, update status and review dates, reset to the 20 default starter tools.

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

All tools live under the KV key `tools` as a JSON array. Each tool has `id`, `name`, `vendor`, `category`, `status`, `description`, `enablementUrl`, `contact`, `nextReview`, `notes`, and `ts`. On first load, admin.html auto-saves the 20 starter tools if the registry is empty. The shared starter tool list lives in `data.js` and is loaded by all three pages.

## Extension ideas

- Approval workflow (request review, pending state, approval notes)
- Usage tracking (teams report which tools they use and how often)
- Cost tracking per tool (licence cost, seats, renewal date)
- Rating or satisfaction score from users
- Integration request form linked from each tool page
- Automated review reminders sent via email when a review date passes
- Comparison view for tools in the same category
- RSS or Slack webhook when a tool status changes

## Extend this app

Copy this prompt into Claude alongside all source files:

```
I have a merebase multi-page app - an AI tool registry for organisational AI adoption.

merebase injects window.MB = { app_id, api } before </head> on every HTML page.
The app is deployed as a zip: index.html, tool.html, admin.html, style.css, data.js.
All tools are stored as a JSON array under the KV key "tools".
Each tool: { id, name, vendor, category, status, description, enablementUrl, contact, nextReview, notes, ts }
Status values: "approved", "experimental", "rejected"
Categories: Assistant, Coding, Writing, Image, Video, Voice, Research, Productivity, Automation
data.js defines STARTER_TOOLS (20 pre-configured tools) used as defaults on first load.
KV API:
  GET  [api]/data?app_id=[id]&key=[k]       - read a value
  POST [api]/data {app_id, key, value}       - write a value
All calls use credentials:'include'.

Here is the current source:
[paste each file with a clear label between them]

I want to [describe what you want to add or change].

The app must remain a zip with index.html at root. No build step.
```
