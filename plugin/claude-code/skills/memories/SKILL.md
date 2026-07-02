---
name: memories
description: Router into Memories.ai video intelligence. Use whenever the user wants to find, add, understand, transcribe, or discover videos — search and recall their own indexed library, index new videos from a URL, read transcripts and scene captions, ask questions about any video/image URL, transcribe audio, or search public / live-web videos. Triggers e.g. "find the soccer goals in my videos", "index this URL and search it", "what is said about pricing in video X", "summarize this YouTube link", "transcribe this recording", "find TikToks about Y".
---

# Memories

One entry point to Memories.ai through the `memories` MCP server. This page is a **router**: read the
invariants, use the decision table to pick the right tool family, then open the matching
`reference/` file for exact parameters and the `examples/` file for a full walkthrough.

## Invariants (always apply)

- **Namespace (`unique_id`)** — Private-library tools (`search_videos`, `search_audio_transcripts`,
  `list_videos`, `get_*`, `upload_video_from_url`, `delete_videos`) are isolated per namespace. Pass
  the right `unique_id`; it must match the one used at upload. Call `list_unique_ids` when unknown;
  omit only for the account default. Analyze / transcribe / scrape / web-search work on arbitrary
  URLs and are **not** namespaced.
- **Surface matters** — "the user's own library" vs "an arbitrary URL" vs "public / live web" are
  different tools with different data. Don't substitute one for another (see the table).
- **Cite** every library result as `videoNo` + start/end time.
- **Confirm destructive actions** — `delete_videos` is irreversible; confirm scope + `unique_id` first.
- **Summarize, never dump raw JSON.** If a tool errors or returns nothing, say so; never fabricate.

## Decision table — pick the surface, then read the reference

| The user wants to… | Tools | Read |
|---|---|---|
| Search / ask about **their own indexed videos** | `search_videos`, `search_audio_transcripts`, `list_videos`, `get_video_metadata`, `get_video_caption`, `get_audio_transcription`, `list_unique_ids` | [reference/search-and-recall.md](reference/search-and-recall.md) |
| **Add** a video to the library (public URL), or delete | `upload_video_from_url`, `delete_videos` | [reference/ingest-and-manage.md](reference/ingest-and-manage.md) |
| Ask about / **transcribe an arbitrary URL** | `analyze_video`, `analyze_image`, `transcribe_audio`, `scrape_social_video` | [reference/analyze-and-transcribe.md](reference/analyze-and-transcribe.md) |
| Find **public / live-web** videos | `search_public_videos`, `search_web_videos` | [reference/discovery.md](reference/discovery.md) |

Full signatures for all 15 tools: [reference/tool-reference.md](reference/tool-reference.md).

## Quick Start — recall from the library (most common)

1. **Find the namespace** (if unknown): `list_unique_ids` → pick a `unique_id`.
2. **Search**: `search_videos(search_param, unique_id, search_type=BY_CLIP)` → ranked clips with
   `videoNo` + start/end times.
3. **Drill in** (exact words / visuals): `get_audio_transcription` or `get_video_caption` on a `videoNo`.
4. **Answer**: summarize and cite each source as `videoNo` + timestamp.

## Tool index (one-liners)

**Library — read:** `search_videos` (semantic) · `search_audio_transcripts` (exact text) ·
`list_videos` · `get_video_metadata` · `get_video_caption` (visual) · `get_audio_transcription`
(spoken) · `list_unique_ids`.
**Library — write:** `upload_video_from_url` (public URL) · `delete_videos`.
**Analyze any URL:** `analyze_video` · `analyze_image` · `transcribe_audio` · `scrape_social_video`.
**Discover:** `search_public_videos` (pre-indexed) · `search_web_videos` (live web, slow).

## Examples (end-to-end walkthroughs)

- [examples/recall-from-library.md](examples/recall-from-library.md) — "find the soccer goals"
- [examples/ingest-then-search.md](examples/ingest-then-search.md) — index a URL, then search it
- [examples/analyze-a-url.md](examples/analyze-a-url.md) — summarize a video by URL
- [examples/web-discovery.md](examples/web-discovery.md) — agentic live-web search
