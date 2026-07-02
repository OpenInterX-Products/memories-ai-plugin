# Example — analyze a video by URL

**User:** "Summarize this video: https://example.com/talks/keynote.mp4"

The video is **not** in the library, so use `analyze_video` (not the library tools).

### Call

```
analyze_video(
  video_url="https://example.com/talks/keynote.mp4",
  prompt="Summarize this talk in 5 bullet points and list any product names mentioned.",
  model="gemini:gemini-2.5-flash"
)
→ { text: "...", model: "...", usage: {...} }
```

- `model` **must** carry a provider prefix (`gemini:` / `nova:` / `qwen:`); a bare id silently fails.
- memories.ai fetches the URL, so it must be public `https`.

### Answer

Return the model's `text`, formatted for the user (don't dump the raw object).

**Variants**
- Image instead of video → `analyze_image` (also allows the `gpt:` prefix).
- Just need the spoken words with timestamps → `transcribe_audio(audio_url=...)`.
- A public social URL (YouTube/TikTok/IG/X) and you want metadata/transcript → `scrape_social_video`.
- If the video **is** already in the library → prefer `get_video_caption` /
  `get_audio_transcription` / `search_videos` (cheaper than re-analyzing).
