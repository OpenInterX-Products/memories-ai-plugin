---
description: Search and recall the user's own indexed video library (Memories.ai).
argument-hint: what to find
---

Use the Memories Skill and the `memories` MCP server to search the user's PRIVATE indexed video
library for `$ARGUMENTS`. See `skills/memories/reference/search-and-recall.md`.

Behavior:

- Treat `$ARGUMENTS` as a natural-language query (e.g. "soccer goals", "where pricing is discussed").
- If the namespace is unknown, call `list_unique_ids` first, then pass the right `unique_id`.
- Pick the tool: `search_videos` (default; `BY_CLIP` for moments) · `search_audio_transcripts` for
  an exact spoken phrase · `list_videos` to browse/filter.
- For detail on a hit, use the `videoNo` with `get_audio_transcription` or `get_video_caption`.
- If nothing matches, say so and suggest a refinement (broader terms, a different `unique_id`).

Summarize results as a short, scannable list citing `videoNo` + start–end time. Do **not** dump raw JSON.
