---
name: onepatch
description: Use when the user asks about production behavior, telemetry, errors, latency, incidents, or wants the OnePatch agent to investigate something — "what's erroring in prod", "query our traces/logs/metrics", "any open incidents", "ask onepatch to look at X". OnePatch is the AI SRE; this skill covers its MCP tools and the `onepatch` CLI.
---

# OnePatch

OnePatch ingests the org's OpenTelemetry data and runs an always-on SRE agent
over it. Two ways in, same tools either way:

- **MCP server** (`onepatch`, bundled with this plugin): use its available
  tools directly. Connect your OnePatch account through the plugin's sign-in
  prompt. If tools are missing, enable the plugin and start a new Codex task;
  if authentication fails, reconnect the OnePatch account in plugin settings.
  A manually configured server can also sign in with `codex mcp login onepatch`.
- **CLI**: `npm install -g onepatch`, then `onepatch login` (device flow).
  The CLI keeps itself up to date automatically.

## Tools

| MCP tool | CLI equivalent | What it does |
|---|---|---|
| `query_otel` | `onepatch otel query <sql\|->` | ClickHouse SQL over `otel.spans`, `otel.logs`, `otel.metrics`, `otel.histograms` |
| `get_ingest_config` | `onepatch otel ingest-config` | OTLP endpoint + ingest token for sending telemetry |
| `list_incidents` | `onepatch incidents list` | Open/all incidents; filter `--severity P0..P3`, `--waiting-on` |
| `read_incident` | `onepatch incidents read <num>` | One incident's document + timeline |
| `list_chats` / `read_chat` | `onepatch chats list` / `read <id>` | The OnePatch agent's chat sessions |
| `start_chat` / `send_chat_reply` | `onepatch chats start` / `reply` | Delegate an investigation to the OnePatch agent |

## Querying telemetry

Standard ClickHouse SQL. Spans/logs/metrics live in the `otel` database; always
bound queries in time (`WHERE start_time > now() - INTERVAL 1 HOUR`) and limit
rows. Explore schema first with `DESCRIBE otel.spans` when unsure.

## Delegating vs. querying

Run your own SQL for quick lookups. For open-ended investigation ("why is
checkout slow since the deploy?"), `start_chat` hands it to the OnePatch agent,
which has the org's full workspace, monitors, and history; poll with
`read_chat` or just give the user the chat link it returns.
