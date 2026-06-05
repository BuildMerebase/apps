# Project tracker

Projects contain tasks. Each project is an entity. Each task is an entity. A relation of type `has_task` links project to task. Completing a task updates its entity. Deleting a project cascades - removes all linked task entities and their relations.

**API:** Entity + Relations  
**Files:** `index.html`

> You can open this app in a browser locally to preview the design. Without merebase as a backend, data runs in memory only and resets on every refresh or page navigation. Deploy to merebase to see full functionality.

## How the data is stored

- Projects: entities of `type: "project"` with `data: { description, status, color, created }`
- Tasks: entities of `type: "task"` with `data: { done, created }`
- Link: a relation `from_id: projectId, to_id: taskId, relation: "has_task"`

Loading a project's tasks means: get all relations where `from_id` is the project, then look up the task entities by `to_id`.

## Extension ideas

- Due dates on tasks with overdue indicators
- Task priority levels
- Assign tasks to team members by name or email
- Project completion percentage and burndown view
- Comments or notes per task
- Milestones grouping sets of tasks
- Export project summary as text

## Extend this app

Copy this prompt into Claude alongside the `index.html` source:

```
I have a merebase single-page app - a project and task tracker.

merebase injects window.MB = { app_id, api } before </head>.
Projects are entities (type: "project"). Tasks are entities (type: "task").
They are linked by a relation (has_task) from project to task.
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
