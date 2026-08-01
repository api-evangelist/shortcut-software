---
name: Plan an iteration (sprint)
description: Create an Epic and an Iteration, add Stories, then review the iteration's Stories.
api: openapi/shortcut-software-openapi-original.json
operations: [createEpic, createIteration, createStory, listIterationStories]
---

# Plan an iteration (sprint) in Shortcut

Stand up a sprint: an Epic to hold the goal, an Iteration to time-box it, and Stories placed into both.

## Auth & conventions
- Base URL `https://api.app.shortcut.com`, paths under `/api/v3`.
- Headers: `Shortcut-Token: <token>`, `Content-Type: application/json`.
- Paginated reads use `page` / `page_size` (max 250); responses carry `data`, `next`, `total`.

## Steps
1. **Create the Epic** — `createEpic` (`POST /api/v3/epics`) with `name` (and optional `objective_ids`, `milestone_id`). Keep the Epic `id`.
2. **Create the Iteration** — `createIteration` (`POST /api/v3/iterations`) with `name`, `start_date`, `end_date`. Keep the Iteration `id`.
3. **Add Stories** — `createStory` (`POST /api/v3/stories`) for each item, setting `epic_id` and `iteration_id` to the ids from steps 1–2 (or `createMultipleStories` for a batch).
4. **Review scope** — `listIterationStories` (`GET /api/v3/iterations/{iteration-public-id}/stories`) to confirm the Stories now belong to the iteration.

## Notes
- Iterations must be enabled for the workspace (`enableIterations`) if not already on.
- Respect the 200 req/min limit when creating many Stories — batch endpoints reduce request count.
