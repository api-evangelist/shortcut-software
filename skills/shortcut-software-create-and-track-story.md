---
name: Create and track a Story
description: Create a Shortcut Story, add tasks and a comment, then update its workflow state.
api: openapi/shortcut-software-openapi-original.json
operations: [getCurrentMemberInfo, createStory, getStory, updateStory, createStoryComment, createTask]
---

# Create and track a Story in Shortcut

Use the Shortcut REST API v3 to create a Story (issue), enrich it, and move it through its workflow.

## Auth & conventions
- Base URL: `https://api.app.shortcut.com`, all paths prefixed `/api/v3`.
- Send `Shortcut-Token: <token>` and `Content-Type: application/json` on every request. HTTPS only.
- Rate limit: 200 requests/minute (429 on exceed). No idempotency key — do not blind-retry writes.
- Errors return `{ "message": ..., "tag": ... }` as `application/json` (not RFC 9457).

## Steps
1. **Identify the caller** — `getCurrentMemberInfo` (`GET /api/v3/member`) to resolve the requesting member and default workspace.
2. **Create the Story** — `createStory` (`POST /api/v3/stories`) with at least `name`; set `workflow_state_id`, `group_id` (owning team), `epic_id`, or `iteration_id` to place it. Capture the returned Story `id`.
3. **Add tasks** — `createTask` (`POST /api/v3/stories/{story-public-id}/tasks`) for each checklist item.
4. **Comment** — `createStoryComment` (`POST /api/v3/stories/{story-public-id}/comments`) to record context.
5. **Advance state** — `updateStory` (`PUT /api/v3/stories/{story-public-id}`) to change `workflow_state_id`, owners, or estimate.
6. **Verify** — `getStory` (`GET /api/v3/stories/{story-public-id}`) to confirm the final state.

## Notes
- To batch, prefer `createMultipleStories` / `updateMultipleStories` over looping single writes against the rate limit.
