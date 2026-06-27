# Example — recall from the library

**User:** "Find the soccer goals in my videos."

### 1. Find the namespace (only if unknown)

```
list_unique_ids()
→ { unique_ids: ["sports-2026", "default"] }
```

Pick the one that fits ("sports-2026"). If it returns an empty list, stop and tell the user there
are no indexed videos under this key — do not guess another namespace.

### 2. Semantic search

```
search_videos(
  search_param="soccer goal",
  search_type="BY_CLIP",
  unique_id="sports-2026",
  top_k=10
)
→ ranked clips, each with videoNo + start/end times
```

### 3. (Optional) Drill into a hit

To quote what the commentator said over a goal:

```
get_audio_transcription(video_no="vid_123", unique_id="sports-2026")
```

### 4. Answer (summarize + cite — never raw JSON)

> Found 3 soccer-goal moments in **sports-2026**:
> - **match-final.mp4** (`vid_123`) — goal at **12:04–12:11**
> - **highlights.mp4** (`vid_777`) — goal at **00:42–00:49**
> - **training.mp4** (`vid_902`) — practice goal at **05:18–05:25**

**Notes**
- Used `BY_CLIP` because the user wants moments, not whole videos.
- Passed the `unique_id`; a missing/mismatched namespace returns empty.
- For an exact spoken phrase instead, use `search_audio_transcripts`.
