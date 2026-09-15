> **Generated repository.** Source of truth: [`1patch/cli`](https://github.com/1patch/cli) (`plugins/codex`). Do not open PRs here.

# OnePatch for Codex

[OnePatch](https://onepatch.dev) is the AI SRE. Connect your OnePatch workspace
to Codex to query traces, logs, and metrics, read incidents, and ask the
OnePatch agent to investigate production problems.

## Install

Add the public OnePatch marketplace and install the plugin:

```sh
codex plugin marketplace add 1patch/codex-plugin
codex plugin add onepatch@onepatch
```

Connect your OnePatch account when prompted, then start a new Codex task.
The plugin bundles the remote MCP server at `https://app.onepatch.dev/mcp`;
no CLI, API key, or manual config edit is required for its tools.
You need a OnePatch account and workspace. [Get started](https://docs.onepatch.dev/getting-started).

In the Codex app, browse the OnePatch marketplace in Plugins after adding it.
This repository is a public install source. Appearance in OpenAI's public
Plugins Directory requires separate review and publication.

## Try it

- "What production incidents need my attention?"
- "Find the top errors in the last hour."
- "Ask OnePatch to investigate checkout latency."

Queries and incident reads are scoped to your organization. Starting a chat
or sending a reply sends your message to the OnePatch agent and can initiate
work in your workspace. [MCP tools and authentication](https://docs.onepatch.dev/mcp-server).

## Optional CLI and incident digest

To connect every supported coding agent on your machine:

```sh
npm install -g onepatch
onepatch install
```

The plugin includes an optional `UserPromptSubmit` hook that displays a cached
open-incidents digest. It requires a signed-in `onepatch` CLI and approval of
the plugin hook in Codex. The MCP tools work independently of this hook.

## Updates

```sh
codex plugin marketplace upgrade onepatch
codex plugin add onepatch@onepatch
```

Start a new task to pick up updated skills and tools.

## Support

[Documentation](https://docs.onepatch.dev) · [Contact support](mailto:support@onepatch.dev) ·
[Privacy](https://onepatch.dev/privacy) · [Terms](https://onepatch.dev/terms)
