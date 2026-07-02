# Tool Reference

Exact signatures for the 15 `memories` MCP tools. Parameters marked `?` are optional. The MCP
server advertises the authoritative input schema at runtime; this is the working reference.

## Library — read (private, namespaced)

### `search_videos(search_param, search_type?, top_k?, filtering_level?, unique_id?, video_nos?, tag?, datetime_taken?, latitude?, longitude?)`
Semantic search over the caller's private indexed library.
- `search_param` — natural-language query.
- `search_type` — `BY_CLIP` (default, moments) · `BY_VIDEO` (whole video) · `BY_AUDIO` (spoken) ·
  `BY_CAPTION` (on-screen/visual) · `BY_IMAGE` (private image library).
- `top_k` (default 10, 1–1000) · `filtering_level` (`low`|`medium`|`high`) · `unique_id` · `video_nos`
  (restrict, ≤100) · `tag` · `datetime_taken` · `latitude`/`longitude`.
- Returns: ranked clips with `videoNo` + start/end times.

### `search_audio_transcripts(query, page?, page_size?)`
Exact-text (substring) search over spoken transcripts. `page` (default 1) · `page_size` (default 50, ≤500).

### `list_videos(page?, size?, unique_id?, video_name?, video_nos?, status?, tag?)`
List indexed videos. `status`: `PARSE` (ready) · `UNPARSE` (indexing) · `FAILED`. `size` ≤200.

### `get_video_metadata(video_no, unique_id?)`
One video's metadata + indexing status. Poll after upload until `PARSE`.

### `get_video_caption(video_no, unique_id?)`
Scene-by-scene **visual** captions (what is seen), with times.

### `get_audio_transcription(video_no, unique_id?)`
**Spoken-word** transcript (what is said).

### `list_unique_ids()`
List namespaces in the account. No parameters.

## Library — write (private, namespaced)

### `upload_video_from_url(url, unique_id?, tags?, video_transcription_prompt?, datetime_taken?, camera_model?, latitude?, longitude?, wait_for_indexing?, timeout_seconds?)`
Index a public **https** video (mp4/m3u8); memories.ai fetches the URL. Returns `videoNo`.
- `wait_for_indexing` (default true) — poll until `PARSE`/`FAIL` or `timeout_seconds` (default 180, ≤540).
- `tags` (≤20; an `api` tag is added automatically) · `datetime_taken` (`yyyy-MM-dd HH:mm:ss`) ·
  `camera_model` · `latitude`/`longitude` · `video_transcription_prompt` (steer understanding).
- Search later with the **same** `unique_id`.

### `delete_videos(video_nos, unique_id?)`
**Destructive, irreversible.** Delete 1–100 videos by `videoNo`. Idempotent (unknown / other-namespace
`videoNo`s are silent no-ops). Use the same `unique_id` the videos were indexed under.

## Analyze any URL (not namespaced)

### `analyze_video(video_url, prompt, model?, mime_type?, temperature?, max_tokens?)`
Vision-language Q&A over a video at a public https URL. `model` requires a provider prefix:
`gemini:` / `nova:` / `qwen:` (default `gemini:gemini-2.5-flash`). `temperature` 0–2 · `max_tokens`.
Returns `{ text, model, id, usage }`.

### `analyze_image(image_url, prompt, model?, mime_type?, temperature?, max_tokens?)`
Same for an image URL. `model` prefix: `gemini:` / `nova:` / `qwen:` / `gpt:` (gpt is image-only).

### `transcribe_audio(asset_id | audio_url, model?, speaker?)`
Whisper STT → timestamped segments. Provide **exactly one** of `asset_id` (existing `re_…` asset) or
`audio_url` (public https; this server downloads + uploads it). `model` (default `whisper-1`) ·
`speaker` (default false; labels speakers, doubles the price).

### `scrape_social_video(video_url, platform, data?, channel?)`
Fetch `metadata` (default) or `transcript` for one public social video. `platform`: `youtube` ·
`instagram` · `tiktok` · `twitter` (URL host must match). `channel`: `apify` (default, richest, incl.
download links) · `rapid` · `memories.ai`.

## Discover (not namespaced)

### `search_public_videos(search_param, type, search_type?, top_k?, filtering_level?)`
Semantic search over a **pre-indexed** public library. `type`: `TIKTOK` · `YOUTUBE` · `INSTAGRAM`.
`search_type`: `BY_VIDEO` (default) · `BY_AUDIO`.

### `search_web_videos(query, platforms?, max_results?, time_frame?, max_steps?, enable_clarification?)`
**Agentic, real-time** web search (YouTube/TikTok/Instagram/X). Returns `{ answer, video_references[], … }`.
Up to ~5 min. `platforms` (subset of youtube/tiktok/instagram/twitter) · `max_results` (default 10, ≤50) ·
`time_frame` (`past_24h`|`past_week`|`past_month`|`past_year`) · `max_steps` (default 10, ≤20) ·
`enable_clarification` (may return a clarifying question instead of results).
