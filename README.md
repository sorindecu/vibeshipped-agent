# Vibe Shipped agent plugin & MCP server

[Vibe Shipped](https://vibeshipped.io) is a daily, sourced directory of real apps people shipped with AI coding tools (Cursor, Claude Code, Lovable, Bolt, Replit, v0, Windsurf and more), plus AI build tools with dated official pricing, coding-agent harnesses and frameworks, revenue rankings (The Vibe Shipped 50) and a what's-new feed. Every entry links to its public source.

This repo holds the plugin, extension and config files so you can use Vibe Shipped from your coding agent. It does not contain the site source.

**MCP endpoint:** `https://vibeshipped.io/api/mcp` (Streamable HTTP, no auth, v1.8.0, 17 tools)
**Docs:** https://vibeshipped.io/agents

## Install

| Client | One line |
|---|---|
| Claude Code (plugin: MCP + skill) | `claude plugin marketplace add sorindecu/vibeshipped-agent && claude plugin install vibeshipped@vibeshipped` |
| Claude Code (MCP only) | `claude mcp add --transport http vibeshipped https://vibeshipped.io/api/mcp` |
| Cursor | [Install in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=vibeshipped&config=eyJ1cmwiOiJodHRwczovL3ZpYmVzaGlwcGVkLmlvL2FwaS9tY3AifQ==) or add `{ "mcpServers": { "vibeshipped": { "url": "https://vibeshipped.io/api/mcp" } } }` to `.cursor/mcp.json` |
| Codex CLI | `codex mcp add vibeshipped --url https://vibeshipped.io/api/mcp` |
| Gemini CLI | `gemini extensions install https://github.com/sorindecu/vibeshipped-agent` |
| goose | `goose session --with-streamable-http-extension https://vibeshipped.io/api/mcp` (persistent config: [`goose/goose-extension.yaml`](goose/goose-extension.yaml)) |
| Hermes Agent | add the `mcp_servers.vibeshipped` block below to `~/.hermes/config.yaml` |
| VS Code | `code --add-mcp '{"name":"vibeshipped","type":"http","url":"https://vibeshipped.io/api/mcp"}'` (workspace config: [`vscode/mcp.json`](vscode/mcp.json)) |

Inside Claude Code you can also run `/plugin marketplace add sorindecu/vibeshipped-agent` then `/plugin install vibeshipped@vibeshipped`.

## Hermes Agent

Add this to `~/.hermes/config.yaml`, then restart Hermes (tools show up as `mcp_vibeshipped_<tool>`):

```yaml
mcp_servers:
  vibeshipped:
    url: "https://vibeshipped.io/api/mcp"
    supports_parallel_tool_calls: true
    tools:
      exclude: [submit_app, subscribe_newsletter]
      resources: false
      prompts: false
```

Drop the `exclude` line if you want the two write tools. Install the skill too:

```bash
hermes skills install well-known:https://vibeshipped.io/.well-known/skills/vibeshipped
```

Results come back as JSON: the text block is an exact copy of `structuredContent`, with a one-line `summary` first, so Hermes keeps one copy instead of two.

## Tools (16)

Read-only: `whats_new`, `search_apps`, `get_app`, `latest_apps`, `list_tools`, `get_tool`, `compare_tools` (2-4 build tools, harnesses or agent tools side by side), `recommend_stack` (ranks tools/harnesses for an app idea with deterministic, documented scoring), `top_tools`, `get_vibe50`, `list_agent_tools`, `list_harnesses`, `get_harness`, `get_sponsorship_info`.

A2A agent card: https://vibeshipped.io/.well-known/agent-card.json

Writes: `submit_app` (adds an app to a review queue) and `subscribe_newsletter` (only with the user's own email).

## Layout

| Path | Used by |
|---|---|
| `.claude-plugin/marketplace.json`, `.claude-plugin/plugin.json` | Claude Code plugin marketplace + plugin |
| `.mcp.json` | MCP server for the Claude Code plugin and Cursor directory |
| `skills/vibeshipped/SKILL.md` | Agent skill (Claude Code, Cursor) |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI extension |
| `goose/goose-extension.yaml` | goose config snippet |
| `vscode/mcp.json` | VS Code workspace MCP config |
| `registry/server.json` | Official MCP Registry entry (`io.github.sorindecu/vibeshipped`) |
| `smithery/smithery.yaml` | Smithery reference |
| `deeplinks.txt` | Cursor and VS Code install deeplinks |

## Data

The full directory is an open dataset under CC BY 4.0: https://vibeshipped.io/data/apps.json and https://vibeshipped.io/data/apps.csv.

## License

The config and manifest files in this repo are MIT licensed (see [LICENSE](LICENSE)). Vibe Shipped data is CC BY 4.0.

## Changelog

- **v1.8.0** (2026-10-27): new `find_tool_for_task` (task, optional client): top 3 MCP servers, agents or skills verified working, each with source, last verified date and a paste-ready install snippet. Backed by the new directory at https://vibeshipped.io/directory (remote MCP servers probed daily with initialize and tools/list only). 17 tools.
- **v1.7.1** (2026-10-22): Hermes Agent support. Tool results are JSON (text = exact copy of `structuredContent`, with a `summary` line), smaller default list sizes with `limit` on `list_agent_tools` and `list_harnesses`, and a well-known skills index at https://vibeshipped.io/.well-known/skills/index.json.
- **v1.5.0** (2026-10-19): new `recommend_stack` and `get_tool`; `compare_tools` now takes 2-4 slugs across build tools, harnesses and Agent Toolbox entries. A2A agent card published. 16 tools.
- **v1.4.0**: 14 tools.
