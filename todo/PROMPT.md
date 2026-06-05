# To-do list

A shared to-do list. Anyone with access to the app sees the same list. Add tasks, check them off, filter by status. Data persists across sessions and devices.

**API:** KV  
**Files:** `index.html`

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

All tasks live under a single KV key (`tasks`) as a JSON array. Each task is an object with `id`, `text`, `done`, and `ts` fields. Writes use `POST /data`, reads use `GET /data`.

## Extension ideas

- Per-user lists using the logged-in user's email as part of the key
- Due dates with overdue highlighting
- Priority levels (high / normal / low) with sorting
- Categories or labels with colour coding
- Assign tasks to team members by name
- Activity log showing who checked what off and when

## Extend this app

Copy this prompt into Claude alongside the `index.html` source:

```
I have a merebase single-page app - a shared to-do list.

merebase injects window.MB = { app_id, api } before </head>.
All tasks are stored as a JSON array under the KV key "tasks".
KV API:
  GET  [api]/data?app_id=[id]&key=[k]       - read a value
  POST [api]/data {app_id, key, value}       - write a value
  POST [api]/data/delete {app_id, key}       - delete a key
All calls use credentials:'include'.

Here is the current source:
[paste index.html here]

I want to [describe what you want to add or change].

Keep it as a single index.html. Do not add a build step.
```
