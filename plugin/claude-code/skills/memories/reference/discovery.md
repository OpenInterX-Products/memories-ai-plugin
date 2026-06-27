# Discover — public & live-web videos

Finding videos the user does **not** own. Not namespaced. Signatures: see
[tool-reference.md](tool-reference.md).

## Which one

| | `search_public_videos` | `search_web_videos` |
|---|---|---|
| Source | Pre-indexed public library | Live web, real time |
| Speed | Fast | Slow (agentic, up to ~5 min) |
| Coverage | One platform per call | Multi-platform, current |
| Returns | Ranked public videos | Synthesized `answer` + `video_references[]` |
| Use when | Quick lookup in a known platform's catalog | "what's the latest…", up-to-date / cross-platform |

Default to `search_public_videos` for speed; reach for `search_web_videos` when freshness or
web-wide coverage matters and the latency is acceptable.

## `search_public_videos`

- `type` (required): `TIKTOK` · `YOUTUBE` · `INSTAGRAM` — one platform per call.
- `search_type`: `BY_VIDEO` (default, visual) · `BY_AUDIO` (spoken).
- `top_k` (default 10) · `filtering_level` (`low`/`medium`/`high`).

## `search_web_videos`

- `query` (required), natural language.
- `platforms` — subset of `youtube`/`tiktok`/`instagram`/`twitter`; omit to let the agent decide.
- `time_frame` — `past_24h` · `past_week` · `past_month` · `past_year` (recency window).
- `max_results` (default 10, ≤50) · `max_steps` (default 10, ≤20, agent loop budget).
- `enable_clarification` — if true, the tool may return a clarifying question (`needs_clarification`)
  instead of results when the query is ambiguous; relay it to the user.

## Gotchas

- Tell the user `search_web_videos` may take minutes; don't retry impatiently.
- It returns a synthesized answer **plus** references — surface both, and cite the references.
- These are public results — never present them as the user's own library content.
