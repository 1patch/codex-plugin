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

Standard ClickHouse SQL. Use the schema in the `query_otel` tool description.
Always bound spans by `start_ts` and logs, metrics, and histograms by `ts`
(for example, `WHERE start_ts >= now() - INTERVAL 1 HOUR`). Limit result rows.
For dated demo data, use the sample's explicit UTC window instead of the last
hour. Report empty results honestly; never invent observations or causes.

## Delegating vs. querying

Run your own SQL for quick lookups. For open-ended investigation ("why is
checkout slow since the deploy?"), `start_chat` hands it to the OnePatch agent,
which has the org's full workspace, monitors, and history; poll with
`read_chat` or just give the user the chat link it returns.


## Discovering capabilities and workflows

Check `onepatch <command> --help` for the installed version's exact syntax.
With CLI 0.8 or later, use `onepatch context --json` for a compact workspace
snapshot, `onepatch tools list` for the live tool index, and
`onepatch tools describe <name>` for one schema and its side effects. Invoke
unwrapped server tools with `onepatch tools run <name> --input '<JSON object>'`.
Read-only discovery does not authorize write operations.

Use `onepatch skills list` and `onepatch skills read <id>` to load public
workspace guidance or one of the bundled workflows: `onepatch-investigate`,
`onepatch-deploy-check`, and `onepatch-instrumentation-check`. Load only the
workflow relevant to the task. `onepatch skills install` installs the three
bundled workflows into ~/.agents/skills; it does not overwrite edited skills.
The corresponding MCP tools are `get_context`, `list_skills`, and `read_skill`.

## Waiting for investigations

`onepatch chats start "<investigation request>" --wait --json` waits for the
specific submitted message. Keep the returned chatId/sendId and resume after
a timeout with `onepatch chats wait <chatId> --send-id <sendId> --json`; do not
resend the request. `--timeout` is in seconds (default 300, maximum 1800).
Accepted, pending and running are not completed work. `needs_input` requires
an answer. Exit codes: 0 complete, 1 failure, 2 needs input, 3 timeout,
130 interrupted. Native incident coordinator conversations support read and
reply; follow their incident view rather than waiting on an SDK turn.

CLI 0.8 `--json` emits `{schemaVersion:1,data:...}` when domain data is
available, or `{schemaVersion:1,data:null,content:[...]}` for older tools.
`--raw-json` preserves the old MCP content-block format. MCP clients can read
structuredContent directly, including start/reply receipts and read_chat's
response for a specified sendId.
