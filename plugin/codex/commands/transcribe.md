---
description: Transcribe an audio or video file to timestamped text (Whisper).
argument-hint: audio/video URL
---

Use the Memories Skill and the `memories` MCP server to transcribe the media in `$ARGUMENTS`. See
`skills/memories/reference/analyze-and-transcribe.md`.

Behavior:

- Parse a public **https** audio/video URL → call `transcribe_audio(audio_url=...)`. If the user
  supplies an existing asset id (`re_…`), use `asset_id=...` instead — pass **exactly one** of them.
- Only set `speaker=true` if speaker labels are requested (it doubles the price).
- Return the timestamped segments.

Format the transcript for the user; do **not** dump raw JSON.
