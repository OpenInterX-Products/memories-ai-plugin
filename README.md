# Memories plugin

让用户在 **Claude Code / Codex** 里查询自己的 Memories.ai 视频记忆(搜索、召回、按时间浏览、钻取转写/关键帧)。

## 最终形态

一个 **plugin**(每个宿主一份),内含:**`.mcp.json`(连远程 MCP)+ skills + commands**;**MCP 部署在服务器**(不打进 plugin)。

> 当前为**本地测试**:plugin 的 `.mcp.json` 指向本机 stdio MCP(`mcp-server/dist`)。MCP 服务器开发好后,把 `.mcp.json` 换成远程 `{"type":"http","url":"https://mcp.memories.ai/mcp"}` 即可,plugin 其余不动。

## 结构

```
memories/
├── mcp-server/                # MCP（资源服务器）— 本地 stdio 测试，将来部署为远程
│   └── src/ …                 #   查询层(参考 PA，已验真数据) + 远程化时: HTTP transport + 校验请求 token
├── plugin/
│   ├── claude-code/           # .claude-plugin/plugin.json + .mcp.json + skills/ + commands/
│   └── codex/                 # .codex-plugin/plugin.json  + .mcp.json + skills/
├── .claude-plugin/marketplace.json   # Claude Code 安装入口 → ./plugin/claude-code
└── .agents/plugins/marketplace.json  # Codex 安装入口      → ./plugin/codex
```

## 认证分工(资源服务器模型)

- **宿主**(Claude/Codex) = OAuth 客户端:浏览器登录、存 token、每次请求带上。
- **MCP** = 资源服务器:收请求里的 token → 校验 → 查;**不存 token、不开浏览器**。
- **memories.ai** = 授权服务器(团队提供,复用现有登录)。

本地测试期:MCP 用网关**服务模式** + 测试 `user_id`(见 plugin 的 `.mcp.json` env);**仅本地,不发布**。

## 本地测试

```bash
cd mcp-server && npm install && npm run build
# 安装插件（Claude Code）
claude plugin marketplace add /Users/zephyr/Documents/memories
claude plugin install memories@memories-local
/reload-plugins
# 然后：/memories <关键词>，或让 agent 调 search_memories / list_memories / get_video_details
```

## 现状

| 部分 | 状态 |
|---|---|
| mcp-server 查询层(PA 移植 · 4 工具) | ✅ 完成,真实网关验过真数据 |
| plugin/claude-code(.mcp.json + skills + commands) | ✅ 本地测试版 |
| plugin/codex | ✅ 本地测试版(Codex 具体安装方式待按版本核) |
| mcp-server 远程化(HTTP + 校验请求 token) | ⬜ 待做 |
| memories.ai 授权服务器 + 宿主 OAuth | ⬜ 团队/宿主 |
| portrait 人物子系统(SQLAlchemy 直连库) | ⬜ 另一套传输 |
