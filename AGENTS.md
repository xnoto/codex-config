# Skills and execution boundaries

- Load `context7` when library or framework documentation is needed; do not load it for unrelated work. Use Codex's native skill discovery, or read the installed `skills/context7/SKILL.md` relative to this file if discovery is unavailable.
- Load `context-mode-routing-policy` before using context-mode to process large output, analyze files, or index public web content. The same relative-file fallback applies. If the policy cannot be loaded, use the dedicated task tools instead of context-mode execution.
- Skills are instructions, not tool installation or permission grants. Use only tools present in the current registry. If a required MCP is unavailable, disclose the limitation and use an approved read-only alternative; do not silently install packages, change configuration, or bypass a denied tool.
- Context-mode is an output-management tool, not a security sandbox. Its execution tools are for bounded read-only local inspection only, never mutations, authenticated API calls, secret retrieval, uploads, or work that requires approval through another tool. Prefer the dedicated MCP for repository, cloud, cluster, and documentation operations.
- Never automatically run shell commands returned by doctor/upgrade tools. Installation, upgrades, configuration changes, and service actions require explicit approval of the exact operation and target.

# MCP routing

Select by the named target environment. If it is unspecified, ask before querying or changing anything.

**Hatch** resources: use only `aws-staging`, `aws-prod`, `kubernetes-staging-eks`, `kubernetes-prod-eks`, `argocd-staging-eks`, `argocd-prod-eks`, and `grafana`. `kubernetes-staging-eks` and `kubernetes-prod-eks` are fixed-context Kubernetes gateways for their respective Hatch EKS environments; they are distinct from `makeitwork-kubernetes`. These are separate servers, so their tools carry no extra prefix — Hatch Grafana is the `grafana` server's `query_prometheus`, while the Hatch Kubernetes gateways use the backend's own tool names such as `pods_list`, not `kubernetes-staging-eks_pods_list`.

**Make IT Work Cloud** resources: use the direct Make IT Work Cloud remote servers. Seven use bare integration names — `apify`, `aws-docs`, `context7`, `parallel-search`, `playwright`, `slidespeak`, and `terraform-docs`. Six use the `makeitwork-` prefix: `makeitwork-argocd`, `makeitwork-aws`, `makeitwork-grafana`, and `makeitwork-kubernetes` retain it because their bare names collide with client integrations that represent other environments; `makeitwork-cloudflare` and `makeitwork-gcp` use it to make the Make IT Work Cloud scope explicit. These names target only Make IT Work Cloud resources, not other environments; the prefix is a naming distinction only, not a security boundary. All thirteen are the same kind of remote server at `https://mcp-<integration>.makeitwork.cloud/mcp`, authenticating with `CF-Access-Client-*` headers referenced from the `CF_ACCESS_CLIENT_ID` and `CF_ACCESS_CLIENT_SECRET` environment variables. Each server exposes its backend's own tool names — `makeitwork-grafana`'s `query_prometheus`, `makeitwork-argocd`'s `list_applications`, `makeitwork-kubernetes`'s `pods_list`. AWS itself is one such server, whose tools read `makeitwork-aws`'s `aws___<tool>`; the server is not AWS-specific despite that prefix. `github` is a workstation-local loopback gateway entry at `http://127.0.0.1:8767/mcp`; the gateway alone sources the unexported `GITHUB_MCP_TOKEN` credential, so clients carry no token headers and spawn no subprocess. `hero-ssh` and `codebase-memory` have no external endpoint and stay on internal or client-local transports; `codebase-memory` remains the local derived index covered in its own section below.

`makeitwork-cloudflare` maps to `https://mcp-cloudflare.makeitwork.cloud/mcp`; `makeitwork-gcp` maps to `https://mcp-gcp.makeitwork.cloud/mcp`. The `makeitwork-` prefix scopes the client-side name only; do not prepend it to the endpoint hostname.

**Environment-neutral** tooling also arrives through direct servers: `parallel-search` (web), `context7` (library docs), `aws-docs`, `terraform-docs`, and `apify`. `opentofu-docs` is a standalone server. Each integration is its own `[mcp_servers.<name>]` entry again — bare for the seven, `makeitwork-`-prefixed for the six — so `enabled` toggles it individually; the aggregate `makeitwork` entry no longer exists.

## Tool usage

The load-bearing usage instructions are restated here.

**Web (the `parallel-search` server)**: reach for `web_search` first for factual, current-information, research, comparison, and troubleshooting questions. Its excerpts are meant to be answered from directly — do not fetch every result. Pass multiple `search_queries` in one call rather than chaining. Use `web_fetch` only when the user names a specific URL, you need exact wording, or the excerpts conflict or are clearly insufficient. Use context-mode fetch/index only when a known public page needs substantial processing; do not refetch sufficient excerpts merely to index them.

**Library docs (the `context7` server)**: use for library, framework, SDK, API, and CLI documentation — syntax, configuration, version migration, setup, and library-specific debugging — even for well-known libraries or questions phrased as "latest". Prefer the dedicated `aws-docs`, `terraform-docs`, and `opentofu-docs` integrations for their domains. For OpenCode configuration, use its schema and canonical repository guidance; do not assume a retired OpenCode docs MCP exists. Use Context7 before general web search for library docs, but not for refactoring, business-logic debugging, code review, or general programming concepts.

**AWS docs (the `aws-docs` server)**: `search_documentation` with specific technical terms, then `read_sections` when the table of contents localizes the answer, otherwise `read_documentation`. Paginate long pages with `start_index`. Fall back to `recommend` when repeated searches come up short. Always cite the documentation URL.

**Grafana (the `makeitwork-grafana` and Hatch `grafana` servers)**: timestamps without a timezone offset are interpreted as UTC. Include an offset such as `-05:00`, or use relative syntax like `now-1h`.

**Terraform/OpenTofu docs**: query the registry for current provider and module versions *before* generating configuration, and pin what you generate.

**Apify (the `apify` server)**: pay-per-event with real money, and it returns bulk datasets. Exhaust the web and docs tools first; reach for it only when the target is login-walled or anti-bot (Facebook Marketplace, Google Maps) or when structured listing records are the actual deliverable. Search the store first — a relevant Actor usually exists — prefer Actors with higher usage or ratings, always check an Actor's input schema before running it, and bound every run with result limits (`resultsLimit`/`maxItems`), price filters, and location radius.

## Codebase Memory routing

`codebase-memory` is a local, derived code-discovery index served by the MCP gateway. Use it only for repositories below `~/git`, and index each repository explicitly rather than indexing the parent directory. Keep shared graph-artifact persistence disabled so repository source is not modified. Indexing changes local derived state and requires confirmation. Its local index can be stale or incomplete; use GitHub for exact file reads, remote branch heads, repository writes, and freshness-critical claims.
