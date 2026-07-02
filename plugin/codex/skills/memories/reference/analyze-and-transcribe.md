# Analyze & Transcribe — any URL

Run intelligence over media that is **not** in the library, addressed by URL (or an uploaded asset).
None of these are namespaced. Signatures: see [tool-reference.md](tool-reference.md).

## Library first

If the video is already indexed, prefer the cheaper library tools — `search_videos`,
`get_video_caption`, `get_audio_transcription` — over re-analyzing a URL. Use `analyze_video` only
for videos that are **not** in the library.

## `analyze_video` / `analyze_image`

Vision/image-language Q&A over a public `https` URL (memories.ai fetches it). Returns the model's
text answer (`{ text, model, id, usage }`).

- **`model` needs a provider prefix** — a bare model id silently picks the wrong upstream path:
  - video: `gemini:` / `nova:` / `qwen:` (default `gemini:gemini-2.5-flash`)
  - image: `gemini:` / `nova:` / `qwen:` / `gpt:` (`gpt` is image-only)
- `temperature` 0–2 (upstream default 0.7) · `max_tokens` (upstream default 1000).
- `mime_type` is inferred from the URL extension; override only if the URL has no/odd extension.

## `transcribe_audio`

Whisper speech-to-text → timestamped segments. Provide **exactly one** source:

- `asset_id` — an already-uploaded memories.ai asset (`re_…`), or
- `audio_url` — a public `https` audio/video URL (this server downloads it, uploads it to obtain an
  asset, then transcribes).

`speaker=true` labels segments by speaker (`SPEAKER_00`, …) and **doubles the price** — only set it
when speaker attribution is requested. `model` defaults to `whisper-1`.

## `scrape_social_video`

Fetch `metadata` (default) or `transcript` for **one** public social video.

- `platform`: `youtube` · `instagram` · `tiktok` · `twitter` — the `video_url` host must match the platform.
- `channel`: `apify` (default, richest shape, includes download links) · `rapid` · `memories.ai`
  (response shape differs per channel; pass through as-is).
- Use this for a single known URL; for discovery across many videos use `search_public_videos` /
  `search_web_videos` ([discovery.md](discovery.md)).

## Gotchas

- `model must include a provider prefix` → add `gemini:` etc.
- `provide exactly one of asset_id or audio_url` → don't pass both (or neither).
- URLs must be public `https`; auth-gated or private URLs fail.
