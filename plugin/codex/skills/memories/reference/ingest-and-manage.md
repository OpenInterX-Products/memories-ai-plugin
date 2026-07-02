# Ingest & Manage — the private library

Add videos to the library and remove them. Signatures: see [tool-reference.md](tool-reference.md).

## Indexing a video — `upload_video_from_url`

memories.ai **fetches the URL itself**, so it must be a publicly reachable `https` video (mp4 or
m3u8) with no auth. The tool returns a `videoNo`.

**Indexing states:** `UNPARSE` (still indexing — not searchable yet) → `PARSE` (ready) or `FAIL`.

Two modes:

- **Wait (default, `wait_for_indexing=true`)** — the tool polls until `PARSE`/`FAIL` or
  `timeout_seconds` (default 180, ≤540). On return, `indexed: true` means it's searchable; if it
  returns with `timed_out: true`, indexing is still running.
- **Return immediately (`wait_for_indexing=false`)** — you get the `videoNo` right away with status
  `UNPARSE`; poll `get_video_metadata(video_no, unique_id)` yourself until `status: PARSE` before searching.

**Namespace:** pass a `unique_id` to keep a project's videos separate; you **must** use the same
`unique_id` in `search_videos` afterwards. Optional metadata: `tags` (≤20), `datetime_taken`
(`yyyy-MM-dd HH:mm:ss`), `camera_model`, `latitude`/`longitude`, `video_transcription_prompt`.

### Typical flow

1. `upload_video_from_url(url, unique_id, wait_for_indexing=true)`.
2. If `indexed` is not true, poll `get_video_metadata` until `status: PARSE` (or surface `FAIL`).
3. `search_videos(..., unique_id=<same>)`.

## Deleting — `delete_videos`

**Destructive and irreversible.** Always confirm scope with the user first. Deletes 1–100 `videoNo`s.
**Idempotent:** unknown `videoNo`s, or ones in another namespace, are silent no-ops and the call still
succeeds — so a "success" does not prove anything was actually deleted. Pass the same `unique_id` the
videos were indexed under (omit for the `default` namespace).

## Gotchas

- A private URL, an http (non-https) URL, or one requiring auth will fail — memories.ai can't fetch it.
- Large videos may still be `UNPARSE` when a waited upload returns; keep polling rather than treating
  it as a failure.
- Searching before `PARSE` returns nothing — confirm status first.
