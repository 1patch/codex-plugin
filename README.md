> **Generated repository.** Source of truth: [`1patch/cli`](https://github.com/1patch/cli) (`plugins/codex`). Do not open PRs here.

# OnePatch — Codex plugin

[OnePatch](https://onepatch.dev) is the AI SRE: it ingests your OpenTelemetry
data and runs an always-on agent over your production systems. This plugin
gives Codex an **`onepatch` skill** that teaches the agent to query telemetry
with SQL, read and drive incidents, and delegate investigations to the
OnePatch agent — over the OnePatch MCP server — plus a **UserPromptSubmit
hook** that injects a one-line open-incidents digest into each turn (needs the
`onepatch` CLI; served from a local cache, so prompts are never delayed).

## Install

**Preferred (one command, wires every agent on the machine):**

```sh
npm install -g onepatch
onepatch install
```

That configures the MCP server in `~/.codex/config.toml` and installs this
plugin. **Manual:**

```sh
codex plugin marketplace add 1patch/codex-plugin
codex plugin install onepatch/onepatch
```

and add the MCP server to `~/.codex/config.toml`:

```toml
[mcp_servers.onepatch]
url = "https://app.onepatch.dev/mcp"
```

Sign in with `codex mcp login onepatch`.

## Updates

`onepatch install` is idempotent — re-run it any time to repair or upgrade
(`codex plugin marketplace upgrade onepatch` also works).
