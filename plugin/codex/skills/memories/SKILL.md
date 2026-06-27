---
name: memories
description: Use when the user wants to search, recall, summarize, or answer questions about their own video memories / recordings (their Memories.ai data) — finding moments, "what did I do last week", pulling transcripts or keyframes.
---

# Memories

通过 `memories` MCP 工具查询用户的个人视频记忆。

## 工具

- `search_memories(query, time_range?, top_k?)` — 语义搜视频记忆(按摘要召回)。用于"找/看看关于 X 的记忆"。
- `recall_segments(query, modality, time_range?, top_k?)` — 转写/关键帧细粒度召回(`modality`: `audio` | `visual` | `keyframe`)。
- `list_memories(time_range?, time_order?, top_k?)` — 按时间浏览。
- `get_video_details(video_ids, aspects?, top_k?)` — 按 videoId 钻取转写/关键帧 + 摘要。

## 用法

1. 先 `search_memories`(或按时间 `list_memories`)拿候选;要细节用 `get_video_details` / `recall_segments`。
2. 回答**引用来源**(video id + 时间);工具报错/未授权时如实说明,不编造。
