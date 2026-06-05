# Client manager

A three-level CRM. Clients contain projects. Projects contain interactions. Each level is a separate entity type linked by relations. The most complex data model of the single-file apps.

**API:** Entity + Relations  
**Files:** `index.html`

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

- Clients: entities of `type: "client"` with `data: { status, email, phone, notes }`
- Projects: entities of `type: "project"` with `data: { status, description }`
- Interactions: entities of `type: "interaction"` with `data: { kind, body, ts }` where `kind` is one of note, call, email, meeting
- Client to project: relation `has_project`
- Project to interaction: relation `has_interaction`

Loading a client's projects means querying relations where `from_id` is the client. Loading a project's interactions means querying relations where `from_id` is the project.

## Extension ideas

- Pipeline stages with drag-and-drop between columns
- Deal values and revenue tracking per client
- Document or file links per client or project
- Bulk import clients from CSV
- Filter and search across all clients
- Dashboard with summary stats (active clients, open projects, interactions this week)
- Export client data as CSV or PDF brief

## Extend this app

Copy this prompt into Claude alongside the `index.html` source:

```
I have a merebase single-page app - a three-level CRM (clients, projects, interactions).

merebase injects window.MB = { app_id, api } before </head>.
Three entity types: client, project, interaction.
Relations: client -has_project-> project -has_interaction-> interaction.
Entity API:
  GET  [api]/entities?app_id=[id]&type=[t]            - list entities
  POST [api]/entities {app_id, type, name, data, id?} - save
  POST [api]/entities/delete {id}                     - delete
Relations API:
  GET  [api]/relations?from_id=[id]                   - list relations
  POST [api]/relations {app_id, from_id, to_id, relation} - create
  POST [api]/relations/delete {id}                    - delete
All calls use credentials:'include'.

Here is the current source:
[paste index.html here]

I want to [describe what you want to add or change].

Keep it as a single index.html. Do not add a build step.
```
