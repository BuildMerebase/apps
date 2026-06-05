# Habit tracker

Track daily habits with a streak counter and weekly grid view. Each habit stores a record of which dates it was completed. The streak is calculated from the completion history.

**API:** KV  
**Files:** `index.html`

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

Habits are stored under the KV key `habits` as a JSON array. Each habit object has `id`, `name`, and `log` - where `log` is an object keyed by ISO date string (`YYYY-MM-DD`) with a boolean value. Completion toggling reads the current habits array, updates the log entry, and writes the whole array back.

## Extension ideas

- Multiple users each tracking their own habits (key per user email)
- Weekly or monthly completion percentage charts
- Habit categories (health, work, learning)
- Custom streak goals (e.g. target 5 of 7 days)
- Notes per completion entry
- Archive completed or abandoned habits

## Extend this app

Copy this prompt into Claude alongside the `index.html` source:

```
I have a merebase single-page app - a daily habit tracker.

merebase injects window.MB = { app_id, api } before </head>.
Habits are stored as a JSON array under the KV key "habits".
Each habit has: id, name, and log (object of {YYYY-MM-DD: true}).
KV API:
  GET  [api]/data?app_id=[id]&key=[k]       - read a value
  POST [api]/data {app_id, key, value}       - write a value
All calls use credentials:'include'.

Here is the current source:
[paste index.html here]

I want to [describe what you want to add or change].

Keep it as a single index.html. Do not add a build step.
```
