# Vibe Shipped agent plugin & MCP server

[Vibe Shipped](https://vibeshipped.io) is a daily, sourced directory of real apps people shipped with AI coding tools (Cursor, Claude Code, Lovable, Bolt, Replit, v0, Windsurf and more), plus AI build tools with dated official pricing, coding-agent harnesses and frameworks, revenue rankings (The Vibe Shipped 50) and a what's-new feed. Every entry links to its public source.

This repo holds the plugin, extension and config files so you can use Vibe Shipped from your coding agent. It does not contain the site source.

**MCP endpoint:** `https://vibeshipped.io/api/mcp` (Streamable HTTP, no auth, v1.4.0, 14 tools)
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
| VS Code | `code --add-mcp '{"name":"vibeshipped","type":"http","url":"https://vibeshipped.io/api/mcp"}'` (workspace config: [`vscode/mcp.json`](vscode/mcp.json)) |

Inside Claude Code you can also run `/plugin marketplace add sorindecu/vibeshipped-agent` then `/plugin install vibeshipped@vibeshipped`.

## Tools (14)

Read-only: `whats_new`, `search_apps`, `get_app`, `latest_apps`, `list_tools`, `compare_tools`, `top_tools`, `get_vibe50`, `list_agent_tools`, `list_harnesses`, `get_harness`, `get_sponsorship_info`.
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
