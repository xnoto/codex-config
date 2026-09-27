# codex-config

Canonical Codex user configuration, distributed by `xnoto/dotfiles` as the
`~/.codex` Git external. Source publication, chezmoi installation, client reload,
and functional verification are separate stages.

## Context skills

| Source | Purpose |
| --- | --- |
| `AGENTS.md` | Always-loaded environment routing and execution boundaries |
| `skills/context7/SKILL.md` | On-demand library documentation workflow |
| `skills/context-mode-routing-policy/SKILL.md` | Bounded output processing without bypassing dedicated tools or approvals |

These are instruction-only skills for the existing MCP entries in `config.toml`.
They add no agents, providers, hooks, permission grants, packages, or endpoint
changes. The context-mode policy is deliberately distinct from the upstream
`context-mode` skill name. This repository owns its native instructions; there
is no generated or automatically synchronized copy from `opencode-config`.

### Discovery and compatibility

The existing external installs these files under `~/.codex/skills`. Codex retains
`$CODEX_HOME/skills` as a **deprecated compatibility location**, confirmed in
[upstream skill-root discovery at 99f7758](https://github.com/openai/codex/blob/99f7758a577740f32df3aad53502e948142758a4/codex-rs/ext/skills/src/host_roots.rs).
This scoped change uses that supported route rather than adding another
chezmoi mapping or placing client-specific skills in the shared
`~/.agents/skills` namespace. No `[[skills.config]]` path is added: that setting
selects existing skills for enable/disable, not a new discovery root.

[Current Codex documentation](https://developers.openai.com/codex/skills)
recommends `~/.agents/skills`. If the installed version removes compatibility,
plan a separate dotfiles-owned mapping migration and check cross-client skill
discovery before changing paths. A non-default `CODEX_HOME` also requires an
explicit installation decision. The relative-file fallback in `AGENTS.md` lets
an agent read the policy without pretending automatic skill discovery worked.

### Dependencies and validation

Context7 uses its existing remote MCP entry. Context-mode uses the existing
bare executable on the client's PATH. A configured entry does not prove the
binary, its dependencies, authentication, or runtime tools are available.
Missing tools should result in a reported limitation and bounded read-only
fallback, not automatic installation or maintenance.

The repository's pre-commit hooks check TOML syntax and secret detection;
there is currently no GitHub Actions workflow or skill-runtime test. Static
review can check frontmatter, routing, and safety wording, but cannot prove
installed-client discovery or MCP health. No workstation validation is implied.

After an approved merge, the existing owner-run chezmoi update can distribute
the files; a new Codex session and a controlled discovery/routing check remain
separate owner-run verification. Do not perform installation, restart, or
credentialed probes merely to validate this source change.
