# Team noticeboard

An internal announcement board deployed as a multi-page zip. Admin posts announcements. Team reads them. Each post has its own shareable URL. The board can go on a screen or be bookmarked by the whole team.

**API:** KV  
**Files:** `index.html`, `post.html`, `admin.html`, `style.css`

## Pages

- `index.html` - the board. Category filter, pinned posts section, post cards. Share this URL with your team.
- `post.html?id=...` - single post detail. Each card links here. Has a copy-link button for sharing in Slack.
- `admin.html` - create posts, set category (General / People / Project / Urgent), pin to top, delete, configure board name and tagline.

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

All posts live under the KV key `posts` as a JSON array. Each post has `id`, `title`, `body`, `category`, `author`, `pinned`, and `ts`. Board config (name, tagline) lives under the KV key `config`. Individual post pages look up their post by `id` from the full `posts` array.

## Extension ideas

- Comments on posts (append to a `comments_[postId]` KV list)
- Reactions or emoji responses per post
- Scheduled posts that appear after a set date
- Department-specific boards (separate deployments per team)
- Email notification link in the post footer
- Archive older posts automatically after N days
- Read receipts showing who has seen each post

## Extend this app

Copy this prompt into Claude alongside all source files:

```
I have a merebase multi-page app - a team noticeboard.

merebase injects window.MB = { app_id, api } before </head> on every HTML page.
The app is deployed as a zip with index.html, post.html, admin.html, and style.css.
All posts are stored as a JSON array under KV key "posts".
Board config (name, tagline) is stored under KV key "config".
KV API:
  GET  [api]/data?app_id=[id]&key=[k]       - read a value
  POST [api]/data {app_id, key, value}       - write a value
  POST [api]/data/append {app_id, key, value} - append to a list
  POST [api]/data/delete {app_id, key}       - delete a key
All calls use credentials:'include'.

Here is the current source:
[paste each file with a clear label between them]

I want to [describe what you want to add or change].

The app must remain a zip with index.html at root. No build step.
```
