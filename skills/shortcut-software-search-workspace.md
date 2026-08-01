---
name: Search the workspace
description: Run a cross-entity search and entity-scoped searches over a Shortcut workspace.
api: openapi/shortcut-software-openapi-original.json
operations: [search, searchStories, searchEpics]
---

# Search a Shortcut workspace

Find work across a workspace using Shortcut's search operations, which accept the same query
syntax used in the app's search bar.

## Auth & conventions
- Base URL `https://api.app.shortcut.com`, paths under `/api/v3`.
- Headers: `Shortcut-Token: <token>`, `Content-Type: application/json`.
- Results are paginated (`page_size` up to 250) and return `data`, `next`, `total`.
- A too-broad query can return `MaxSearchResultsExceededError` (400) — narrow it or paginate.

## Steps
1. **Cross-entity search** — `search` (`GET /api/v3/search`) with a `query` to get Stories, Epics, Iterations, Objectives, and Milestones in one call.
2. **Scoped search** — for a single type, call `searchStories` (`GET /api/v3/search/stories`) or `searchEpics` (`GET /api/v3/search/epics`) with `query` + `page_size`.
3. **Page through** — follow the `next` field / increment `page` until `next` is empty.

## Notes
- Query operators mirror the app (e.g. `state:`, `owner:`, `epic:`, `is:story`).
- Other scoped variants exist: `searchIterations`, `searchMilestones`, `searchObjectives`, `searchDocuments`.
