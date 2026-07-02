---
description: Index a video into the user's Memories.ai library from a public URL.
argument-hint: video URL [namespace]
---

Use the Memories Skill and the `memories` MCP server to index the video in `$ARGUMENTS`. See
`skills/memories/reference/ingest-and-manage.md`.

Behavior:

- `$ARGUMENTS` must be a public **https** video URL (mp4/m3u8) → `upload_video_from_url` (memories.ai
  fetches it; waits for indexing by default; if it returns before `PARSE`, poll `get_video_metadata`).
- If a namespace is given, pass it as `unique_id` — searches must reuse the same one.
- Report the `videoNo`, the final status, and the `unique_id` to use when searching it later.

Summarize the outcome; do **not** dump raw JSON.
