# Example — discover videos on the live web

**User:** "Find recent YouTube videos explaining the offside rule."

This is public/web content, not the user's library → discovery tools.

### Fast path — pre-indexed public library

```
search_public_videos(search_param="offside rule explained", type="YOUTUBE", search_type="BY_VIDEO")
→ ranked public videos
```

### Fresh path — agentic live web (slower, ~minutes)

When the user wants the latest or cross-platform results:

```
search_web_videos(
  query="offside rule explained",
  platforms=["youtube"],
  time_frame="past_month",
  max_results=10
)
→ { answer: "...", video_references: [ ... ] }
```

- Warn the user it can take up to ~5 minutes; don't retry impatiently.
- If `enable_clarification=true` and the query is ambiguous, it may return `needs_clarification` —
  relay the question to the user instead of forcing results.

### Answer

Surface the synthesized `answer` **and** the `video_references` (with links). Make clear these are
public results, not the user's own library.
