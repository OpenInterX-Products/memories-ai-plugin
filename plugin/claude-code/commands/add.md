---
description: Index a video into the user's Memories.ai library — from a public URL or a local file.
argument-hint: video URL or local file path [namespace]
---

Use the Memories Skill and the `memories` MCP server to index the video in `$ARGUMENTS`. See
`skills/memories/reference/ingest-and-manage.md`.

Behavior:

- If `$ARGUMENTS` is a public **https** video URL (mp4/m3u8) → `upload_video_from_url` (memories.ai
  fetches it; waits for indexing by default; if it returns before `PARSE`, poll `get_video_metadata`).
- If `$ARGUMENTS` is a **local file path** → `upload_video_from_file` (works only when the MCP server
  runs on the same machine as the file; a remote server can't read local files).
- If a namespace is given, pass it as `unique_id` — searches must reuse the same one.
- Report the `videoNo`, the final status, and the `unique_id` to use when searching it later.

Summarize the outcome; do **not** dump raw JSON.
