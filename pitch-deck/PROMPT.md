# Pitch Deck

A fullscreen startup pitch deck builder and presenter. Three slide templates — title, content (with bullets), and stats (big number/label blocks). Click any text on a slide to edit it in place. Hover a block to delete it. Stores the entire deck as a single KV value.

**API:** KV  
**Files:** `index.html`

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh. Deploy to merebase to see full functionality.

## How the data is stored

The deck is stored under the KV key `deck` as one JSON object with a `slides` array. Each slide has an `id`, a `type` (`title`, `content`, or `stats`), and a `blocks` array. Each block has an `id`, a `kind` (`headline`, `tagline`, `byline`, `intro`, `bullet`, or `stat`), and either a `text` field or `value`/`label` fields (for stat blocks).

On first load with no stored data the sample deck is automatically saved and displayed.

## Extension ideas

- Additional slide templates — quote, two-column, image+text
- Slide background images — base64 or absolute URL per slide
- PDF export — print stylesheet that renders one slide per page
- Presenter view — second window showing current slide plus speaker notes
- Read-only share link — a public URL using a separate KV key
- Slide reorder — drag-and-drop in edit mode
- Transition styles — per-slide animation presets
- Timer overlay — countdown clock for timed pitches

## Extend this app

Copy this prompt into Claude alongside the `index.html` source:

```
I have a merebase single-page app — a fullscreen pitch deck builder and presenter.

merebase injects window.MB = { app_id, api } before </head>.
The deck is stored under the KV key "deck":
  { slides: [ { id, type, blocks: [ { id, kind, text?, value?, label? } ] } ] }
Slide types: title, content, stats.
Block kinds: headline, tagline, byline, intro, bullet, stat.
KV API:
  GET  [api]/data?app_id=[id]&key=[k]   - read a value
  POST [api]/data {app_id, key, value}   - write a value
All calls use credentials:'include'.

Here is the current source:
[paste index.html here]

I want to [describe what you want to add or change].

Keep it as a single index.html. Do not add a build step.
```
