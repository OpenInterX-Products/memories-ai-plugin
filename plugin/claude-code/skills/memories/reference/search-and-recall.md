# Search & Recall — the private library

How to retrieve and answer from the user's own indexed videos. Signatures: see
[tool-reference.md](tool-reference.md).

## Choose the tool

- **Semantic / conceptual** ("clips about soccer goals") → `search_videos`.
- **Exact phrase** ("where someone says 'refund policy'") → `search_audio_transcripts` (substring, not semantic).
- **Browse / filter** ("videos uploaded this week", "still indexing") → `list_videos` (filter by
  `status`, `video_name`, `tag`, `video_nos`).
- **Drill into one video** → `get_audio_transcription` (spoken) or `get_video_caption` (visual), by `videoNo`.

## `search_videos` modality (`search_type`)

| `search_type` | Matches | Use when |
|---|---|---|
| `BY_CLIP` (default) | moments within videos | "find the part where…" |
| `BY_VIDEO` | whole videos | "which videos are about…" |
| `BY_AUDIO` | spoken content | "who talks about…" |
| `BY_CAPTION` | on-screen text / visual scene | "videos showing a whiteboard with…" |
| `BY_IMAGE` | the private image library | image-library lookups |

`filtering_level` (`low`/`medium`/`high`) trades recall for precision — raise it when results are noisy.

## Namespaces

Pass the `unique_id` the videos were indexed under. If unknown, call `list_unique_ids` first.
**A mismatched or missing `unique_id` returns empty even when data exists.** If `list_unique_ids` is
empty, the account has no indexed videos under this key — tell the user, don't guess another namespace.

## Visual vs spoken

- `get_video_caption` → what is **seen** (scene-by-scene visual captions).
- `get_audio_transcription` → what is **said** (speech-to-text).
  Pick based on whether the user asks about imagery or dialogue.

## Pagination

`search_audio_transcripts` uses `page` / `page_size` (≤500); `list_videos` uses `page` / `size` (≤200).
Page through only as far as needed.

## Answering

Return a short, scannable summary. Cite every result as **`videoNo` + start–end time**. Never paste
raw JSON. If nothing matches, say so and suggest a refinement (broader terms, different `unique_id`,
or — for public content — `search_public_videos`).
