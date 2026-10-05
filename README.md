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

## Bounded specialist subagents

Native user agents live in `agents/*.toml` and travel through the existing Git
external to `~/.codex/agents/`; a non-default `CODEX_HOME` still needs an explicit
installation decision. No dotfiles mapping or global config change is required.

| Agent | Use |
| --- | --- |
| `adversarial-code-reviewer` | Independent completed-diff review before a PR or a requested second opinion. |
| `qa-engineer` | Acceptance-to-check coverage, test cases, and documentation adequacy. |
| `docs-writer` | Standalone documentation drafting; not agent policy, skills, or knowledge bases. |
| `infra-security-reviewer` | Infrastructure secret-handling, privilege, exposure, and supply-chain review. |
| `devops-engineer` | DESIGN/CHANGE review of CI, workflows, artifacts, runners, and integration contracts. |
| `release-engineer` | Actual release-contract, version, pin, generated-copy, and delivery-stage review. |
| `cloud-architecture-reviewer` | Preimplementation review of new/material cloud service, state, recovery, scaling, or cost decisions. |

The primary supplies the complete relevant evidence, intent, repository contracts,
producer-consumer context, and validation status. Missing essential inputs return
HOLD or BLOCKED; reviewers do not browse for substitutes. Use fresh reviewer
context and invoke roles selectively, not all seven on every change. Resolve
Critical/High findings or obtain an explicit owner waiver. Verdicts never authorize
merge, publication, deployment, installation, or live mutation. Models inherit the
parent; no chart provider pins are imported.

These native files are independently adapted from the roles in
[opencode-server](https://github.com/makeitworkcloud/charts/tree/70c96e408c6bc0e04532a055c8558a38c2d861be/opencode-server/files/agents).
Each config repository owns its copies; no generator or automatic synchronization
was introduced. The source chart, MCP endpoints, packages, and primary model
settings are unchanged.

The [Codex subagent reference](https://developers.openai.com/codex/subagents)
defines standalone TOML roles. These files inherit model and reasoning settings,
request `sandbox_mode = "read-only"`, set `approval_policy = "never"`, disable web
search, and disable shell, nested agents, connectors, hooks, and skill MCP
dependency installation using documented configuration fields.

Every MCP server currently declared in `config.toml` is explicitly disabled in
each role. This is a named-server deny set, **not a wildcard guarantee**: when
adding/renaming a server, update all seven role files. Project, plugin, or
additional user configuration may introduce other tools. Parent sandbox/approval
settings can affect children; a filesystem sandbox does not constrain remote
MCP mutations. Do not rely on the role default alone as a hard isolation boundary.
See the [configuration reference](https://developers.openai.com/codex/config-reference).

Pre-commit checks TOML syntax and secret detection, not effective native agent
configuration. There is no Actions workflow here. Before using these roles after
an approved merge and owner-run external update, verify the installed client
supports every selected field, discovers all seven roles, inherits the intended
model, and exposes no shell, remote mutation, or nested-agent capabilities. If
restrictions or discovery cannot be verified, stop and report the gap rather than
loosening controls. No installation, restart, or functional verification is claimed.
