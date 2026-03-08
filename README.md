# codex-config

Codex CLI equivalent for MCP setup in `opencode-config`.

## What maps cleanly

- MCP servers: yes (`[mcp_servers.<name>]` in `~/.codex/config.toml`)
- Per-server enabled/disabled: yes (`enabled = true|false`)
- HTTP bearer token auth: yes (`bearer_token_env_var`)

## Plugin parity

Codex CLI does not currently use the same plugin marketplace model as Claude Code/OpenCode.

- `context-mode`: use as MCP server (`context-mode` command), already configured.
- `opencode-mem` / `claude-mem`: no direct Codex plugin equivalent.
  - Closest native option is Codex built-in memories (`~/.codex/memories`).
  - If you want external memory behavior, add a dedicated memory MCP server when you choose one.

## Use with dotfiles

Manage this repo in dotfiles and sync:

- `config.toml` -> `~/.codex/config.toml`

After sync, verify:

```bash
codex mcp list
codex mcp get context-mode
```
