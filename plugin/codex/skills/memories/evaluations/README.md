# Memories Skill Evaluations

Scenario tests for the `memories` skill. These are **not loaded at runtime** — they are a QA /
regression harness used to verify the skill drives correct tool use across model versions.

## Purpose

Ensure the skill makes the agent:

- Pick the right **surface** (own library vs arbitrary URL vs public/web) for the request.
- Pass the correct **namespace** (`unique_id`) and `search_type` for library calls.
- Wait for indexing (`PARSE`) before searching freshly uploaded videos.
- **Cite** results (`videoNo` + timestamp) and **summarize** rather than dump raw JSON.
- Confirm destructive deletes; never fabricate when a tool returns nothing.

## Files

| File | Scenario |
|---|---|
| `recall.json` | Search the user's own library for a concept |
| `ingest-then-search.json` | Index a URL, wait for `PARSE`, then search it |
| `analyze-a-url.json` | Answer a question about a non-library video URL |
| `web-discovery.json` | Discover public/live-web videos (not the library) |

Each file: `{ name, skills, query, expected_behavior[], success_criteria[] }`.

## Running

1. Enable the `memories` plugin and connect the MCP server.
2. Submit the scenario's `query`.
3. Check the agent's tool calls + output against `expected_behavior` and `success_criteria`.
4. Repeat across Haiku / Sonnet / Opus for consistency.

## Writing good criteria

Make them specific and testable.

- **Good:** "Calls `search_videos` with `search_type=BY_CLIP` and the chosen `unique_id`."
- **Bad:** "Searches videos correctly."
