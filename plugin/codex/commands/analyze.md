---
description: Ask a question about, or summarize, a video or image at a public URL.
argument-hint: URL [question]
---

Use the Memories Skill and the `memories` MCP server to analyze the media in `$ARGUMENTS`. See
`skills/memories/reference/analyze-and-transcribe.md`.

Behavior:

- Parse a public **https** URL and an optional question (default: summarize it).
- If the video is already in the user's library, prefer `get_video_caption` /
  `get_audio_transcription` / `search_videos` (cheaper than re-analyzing a URL).
- Otherwise call `analyze_video` (video) or `analyze_image` (image) with a **provider-prefixed**
  `model` (e.g. `gemini:gemini-2.5-flash`; `gpt:` is image-only).
- Return the model's text answer.

Present the answer as readable prose; do **not** dump the raw response object.
