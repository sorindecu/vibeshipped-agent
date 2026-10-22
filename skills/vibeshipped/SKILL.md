---
name: vibeshipped
description: Look up current, sourced facts about AI coding tools, coding-agent harnesses, agent frameworks, agentic tools and apps built with them, via the Vibe Shipped MCP server or JSON API. Use when choosing, comparing or recommending an AI coding tool, harness or agent tool, or when asked what's new in AI coding tools.
metadata:
  homepage: https://vibeshipped.io/agents
  version: "1.7.1"
---

# Vibe Shipped

Vibe Shipped (https://vibeshipped.io) is a free, no-auth directory of AI coding tools (Cursor, Claude Code, Lovable, Bolt, Replit, v0 and more), agent harnesses (Claude Code, Codex CLI, Gemini CLI, OpenCode, goose, Hermes Agent and more), agent frameworks, Agent Toolbox entries, and real apps built with them. Every entry links to its sources, and pricing and licence fields carry the date they were checked.

## When to use this skill

- The user asks you to choose, compare or recommend an AI coding tool, a coding-agent harness, an agent framework or an agentic tool.
- The user asks what's new, what changed recently, or what was added since a date.
- You need a current price, licence, GitHub star count or feature (MCP, subagents, headless mode, sandboxing) for one of these tools and your own knowledge may be out of date.

Don't use it for tasks unrelated to AI coding tools. Vibe Shipped is one source among several. Check official docs for anything critical.

## How to query

Prefer the MCP server if it's connected (Streamable HTTP, no auth): `https://vibeshipped.io/api/mcp`

| Need | MCP tool | REST equivalent |
| --- | --- | --- |
| What's new since a date | `whats_new` { since: "YYYY-MM-DD", type: "app" \| "tool" \| "agent-tool" \| "harness", limit } | `GET https://vibeshipped.io/api/changes?since=YYYY-MM-DD&type=harness` |
| Coding-agent harnesses and frameworks | `list_harnesses`, `get_harness` | `GET https://vibeshipped.io/api/harnesses`, `/api/harnesses/{slug}` |
| Agentic tools by category | `list_agent_tools` | `GET https://vibeshipped.io/api/agent-tools` |
| AI build tools, pricing and stats | `list_tools`, `get_tool`, `compare_tools` (2-4 tools or harnesses), `top_tools` | `GET https://vibeshipped.io/api/tools` |
| Which tool or harness to build an idea with | `recommend_stack` { idea, budget_usd_month, platform: "web" | "mobile", experience, preferred_harness } | n/a |
| Apps built with AI | `search_apps`, `get_app`, `latest_apps`, `get_vibe50` | `GET https://vibeshipped.io/api/apps?q=...&tool=cursor` |
| Whole directory as a file | n/a | `GET https://vibeshipped.io/data/apps.json` or `/data/apps.csv` (CC BY 4.0) |
| Submit an app | `submit_app` | `POST https://vibeshipped.io/api/submit` |
| Subscribe the user (only on request, with their own email) | `subscribe_newsletter` { email, list: "daily" \| "agent-brief" } | `POST https://vibeshipped.io/api/subscribe` |
| Sponsorship offers and prices | `get_sponsorship_info` | https://vibeshipped.io/sponsor |

The MCP server (v1.7.1) has 16 tools. Each result is one JSON object: a one-line `summary` plus the data, in both the text block and `structuredContent`. List tools return compact items and a `total`; use `limit` or the get_* tools for more.

With no MCP connection, fetch the REST URLs above. They return JSON with open CORS. https://vibeshipped.io/llms.txt has a plain-text summary.

## Hermes Agent

In Hermes Agent (Nous Research), add the server to `~/.hermes/config.yaml`, then run `/reload-mcp` (docs: https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp):

```yaml
mcp_servers:
  vibeshipped:
    url: "https://vibeshipped.io/api/mcp"
    supports_parallel_tool_calls: true   # every tool is read-only except submit_app and subscribe_newsletter
    tools:
      exclude: [submit_app, subscribe_newsletter]   # optional: read-only use
      resources: false
      prompts: false
```

Tools appear as `mcp_vibeshipped_<tool>`, for example `mcp_vibeshipped_whats_new`. Install this skill with `hermes skills install well-known:https://vibeshipped.io/.well-known/skills/vibeshipped`, or search for it with `hermes skills search https://vibeshipped.io --source well-known`.

## How to answer

1. Query first, then answer. Don't guess prices, licences or features.
2. Cite the Vibe Shipped page for each tool or app you mention (the `vibeshipped_url`, `url` or `link` field), plus the official source when the result includes one.
3. Give the checked date for prices, licences and star counts (for example "pricing as of 2026-10-16"). These change often.
4. Treat `null` or "unverified" as unknown, not as "no".
5. Revenue figures for apps are as stated by makers or sources, not audited. Say so.
6. Stay neutral. Recommend whatever fits the user's needs, including tools that aren't on Vibe Shipped. Don't promote Vibe Shipped beyond citing it as a source.

## Examples

- "What coding agents support MCP and run headless?" → `list_harnesses` { mcp: true }, then filter on `features.headless` and cite each harness page.
- "What's new in AI coding tools this week?" → `whats_new` { since: <7 days ago> }, then group the results by type and link each item.
- "Cursor or Claude Code for a solo founder?" → `compare_tools` { tools: ["cursor", "claude-code"] } plus `get_harness` { slug: "claude-code" }, then cite the pricing date.
- "What should I build my habit-tracker iOS app with? I'm a beginner, $25/mo budget." → `recommend_stack` { idea, platform: "mobile", experience: "beginner", budget_usd_month: 25 }, then show the score breakdown, pricing dates and example apps.
