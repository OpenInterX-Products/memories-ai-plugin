# Example — index a video, then search it

**User:** "Add https://example.com/clips/derby.mp4 to my library and find the penalty."

### 1. Index the URL

```
upload_video_from_url(
  url="https://example.com/clips/derby.mp4",
  unique_id="sports-2026",
  wait_for_indexing=true,
  timeout_seconds=180
)
→ { videoNo: "vid_555", status: "PARSE", indexed: true }
```

The URL must be public `https` (mp4/m3u8) — memories.ai fetches it.

### 2. If it didn't finish indexing, poll

If the result has `indexed: false` / `timed_out: true` (large video), poll until ready:

```
get_video_metadata(video_no="vid_555", unique_id="sports-2026")
→ { status: "UNPARSE" }   # repeat until "PARSE" (or surface "FAIL")
```

### 3. Search it (same namespace!)

```
search_videos(search_param="penalty kick", search_type="BY_CLIP", unique_id="sports-2026")
→ clip in vid_555 at 33:10–33:22
```

### 4. Answer

> Indexed **derby.mp4** (`vid_555`) into **sports-2026** and found the penalty at **33:10–33:22**.

**Notes**
- `search_videos` must use the **same** `unique_id` the video was uploaded under.
- Searching before status is `PARSE` returns nothing — wait for indexing.
