---
description: Search the live public web for videos (agentic, real-time).
argument-hint: query
---

Use the Memories Skill and the `memories` MCP server to find public videos matching `$ARGUMENTS`.
See `skills/memories/reference/discovery.md`.

Behavior:

- For up-to-date or cross-platform results, use `search_web_videos` (an agentic live-web sweep —
  warn the user it can take up to ~5 minutes; don't retry impatiently).
- For a fast lookup in one platform's pre-indexed catalog, use `search_public_videos` with the right
  `type` (`YOUTUBE` / `TIKTOK` / `INSTAGRAM`).
- Restrict by platform / `time_frame` when the user implies recency or a specific source.

Surface the synthesized answer **and** the video references (with links). Make clear these are public
results, not the user's own library. Do **not** dump raw JSON.
