# Agent guide

This repository holds workload Helm charts and their per-cluster values. Platform services stay in [ThriftyStack/platform](https://github.com/ThriftyStack/platform.git).

## Documentation in Notion

Notion is the source of truth for ThriftyStack documentation. Write documentation changes in Notion. The ThriftyStack/docs GitHub repository ended at commit 74b017f.

Read the relevant Notion page before you change behavior here. In the same task, update that page, or add a page under the matching section. Keep the page's "Last updated by" line. When the line is already there, change the name and the date on it. When the line is missing, add `Last updated by: <name>, YYYY-MM-DD` at the top of the page.

Record each new decision as its own page under [ADRs](https://www.notion.so/400972443c708169b262e47f7c9c56e0).

Never put secrets in Notion. Tokens, passwords, private keys, and customer data stay out of Notion pages. Each person keeps OAuth credentials in the MCP client on their own machine.

### Pages for this repository

Start at [ThriftyStack Docs](https://www.notion.so/400972443c7081fd83efd3e8710fc9a6).

Open the page that matches the change.

- [CI](https://www.notion.so/400972443c7081b39966d2c0eb174498) for delivery and for the validation workflow in this repository.
- [Governance](https://www.notion.so/400972443c7081b291f1d4cf96989474) for how a change is approved.
- [ADRs](https://www.notion.so/400972443c708169b262e47f7c9c56e0) for a decision.
- [Disaster Recovery](https://www.notion.so/400972443c70814e9ebccf1806f12504) when a workload change affects recovery.
- [Systems](https://www.notion.so/400972443c7081658761c83b9fc2de16) as the index of platform systems. Read [Vault](https://www.notion.so/400972443c70813db431fc0bc32215e2) when the change touches secret delivery or an enrollment gate. Read [Authentik](https://www.notion.so/400972443c7081d2885dcba7e839fedf) when the change touches identity. Read [NetBird](https://www.notion.so/400972443c70815fb7b7db1baf120d43) when the change touches network access.

Share the Notion root page with every account that signs in through MCP. An empty tool result means that account cannot see the page.

### Notion MCP setup

The hosted server URL is `https://mcp.notion.com/mcp`. The config files in this repository point at that URL. Those files contain no tokens. Each person signs in once per tool.

#### Cursor

Project config is `.cursor/mcp.json`.

1. Open Customize in the Cursor sidebar.
2. Enable the Notion server.
3. Finish the Notion OAuth flow.

From this repository, run `agent mcp login notion` to start the same login.

Check with `agent mcp list`. The `notion` row shows connected. When the row is not connected, open the Output panel and select MCP Logs. Read the error there.

#### Claude Code

Project config is `.mcp.json`. The Notion server sets `"type": "http"`.

1. Run `claude` in this repository.
2. Trust the workspace when asked.
3. Approve the project MCP server.

Until that approval, `claude mcp list` reports the server as pending approval.

Sign in once with this command.

```bash
claude mcp login notion
```

Inside a session, run `/mcp`, select `notion`, and choose Authenticate.

Check with `claude mcp list`. Connected output contains `Connected`. Output before login contains `Needs authentication`.

`claude mcp add` without `--scope project` stores a private server and leaves `.mcp.json` unchanged. Keep the committed file. Recreate it with `claude mcp add --transport http notion --scope project https://mcp.notion.com/mcp`.

#### Codex

Project config is `.codex/config.toml`. The entry is `[mcp_servers.notion]` and the endpoint key is `url`. Codex reads that file only after you trust the project.

Sign in once from this repository.

```bash
codex mcp login notion
```

Check with `codex mcp list`. After login, the list names `notion`.

`codex mcp add` writes only `~/.codex/config.toml`. Keep the project file as the shared server entry. Leave `bearer_token_env_var` unset in the committed file.
