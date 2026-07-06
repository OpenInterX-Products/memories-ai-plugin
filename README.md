# Memories plugin

Search, recall, and analyze your [Memories.ai](https://memories.ai) video library from
**Claude Code** and **Codex** — find moments, read transcripts and scene captions, index new
videos, ask questions about any video/image, and discover public / live-web videos.

## What it can do

- **Recall your library** — semantic clip/video search, exact-phrase transcript search, browse by time.
- **Drill in** — scene-by-scene visual captions and spoken-word transcripts for any video.
- **Add videos** — index a public https video URL into your library.
- **Analyze any URL** — vision-language Q&A over a video/image, or transcribe audio.
- **Discover** — search pre-indexed public libraries (TikTok/YouTube/Instagram) or the live web.

## Install (Claude Code)

```bash
claude plugin marketplace add OpenInterX-Products/memories-ai-plugin
claude plugin install memories@memories-ai
```

Or use the `/plugin` menu. On first use, a browser opens to sign in to your memories.ai account and
authorize the connection — nothing to run locally.

Then just ask (or use the commands): `/memories:search <query>`, `/memories:add <url>`,
`/memories:analyze <url>`, `/memories:transcribe <url>`, `/memories:web <query>`.

## Install (Codex)

The Codex build lives in `plugin/codex/` (same skills + commands). Install it from this repo as a
marketplace source:

```bash
codex plugin marketplace add OpenInterX-Products/memories-ai-plugin
codex plugin add memories@memories-ai
```

Or install it from the **Plugins** screen in the Codex app. On install a browser opens to sign in to
your memories.ai account and authorize the connection — nothing to run locally.

Then just ask, or type `@` to invoke the Memories plugin and its bundled commands (`search`, `add`,
`analyze`, `transcribe`, `web`).

> OpenAI's official curated Codex plugin directory is invite/curated for now — self-serve submission
> is not open yet, so install via the repo marketplace above until it is.

## How it works

The plugin is **thin**: skill docs + commands + a `.mcp.json` that points at the hosted MCP server
(`https://mcp.memories.ai/mcp`). There is **no local server to run**.

- **Host** (Claude Code / Codex) = OAuth client: opens the browser, stores the token, sends it per request.
- **MCP** = authorization + resource server: issues the token (your memories.ai key sealed inside,
  encrypted) and serves the tools. It never persists your key.
- **memories.ai** = where you log in; your API key lives there.

## Structure

```
.claude-plugin/marketplace.json   # Claude Code marketplace → ./plugin/claude-code
.agents/plugins/marketplace.json  # Codex marketplace       → ./plugin/codex
plugin/
├── claude-code/                  # .mcp.json + skills/ + commands/
└── codex/                        # .mcp.json + skills/ + commands/
```

## Requirements

A [memories.ai](https://memories.ai) account (free tier works). Everything else — the MCP server and
OAuth — is hosted.
