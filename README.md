# fd-vertical-packs
> **Constellation (萬星) product line** · the vertical-pack ecosystem of [FindData](https://www.finddatatech.cloud/products/constellation) — industry entry skills + cloud agent catalog.

Vertical pack assets for the FindData AI platform (`craw.finddatatech.cloud`) and its
MCP registry market (`mcp.finddatatech.cloud`).

- `skills/<name>/SKILL.md` — four industry entry skills (source of truth; registered
  into the registry, installed from the platform Store):
  - `legal-contract-workflow` — 法律-合同 (law-bench MCP)
  - `legal-case-workflow` — 法律-案件 (fd-legal-search-mcp when available)
  - `stock-research-workflow` — 数据-股票 (fd-open-data-mcp + fd-cn-report)
  - `china-macro-brief-workflow` — 数据-中国经济 (fd-open-data-mcp + fd-cn-report)
- `agents.json` — cloud agent catalog (`AGENTS_CONFIG_URL`): three chat-mode pack
  agents. Credentials are env-var references only; no secrets in this repo.
- `registry-groups.json` — reference copy of the platform's role-visibility mapping
  (the live copy is the ConfigMap in fd-infra-deploy).

## ZCode plugin marketplace (`marketplace.json`)

`marketplace.json` at this repo's root declares a plugin marketplace you can add
directly in ZCode: **Settings → Plugin Marketplace → Add → Add Plugin Marketplace**
→ paste `https://github.com/FindDataTechnology/fd-vertical-packs`.

- `plugins/finddata-coding-crew/` — the v2 five-role crew (planner, explorer,
  implementer, merger, verifier) **plus** the three roles team-v3 adds: `relay`
  measures the model path and hands out the round's concurrency width,
  `contractor` pins the public interface to one machine-readable contract,
  `sentry` gates the merge on artifacts that actually exist.

  **One agent directory cannot serve both rosters.** team-v3 narrows some grants
  (merger `[Read, Write]`, verifier/sentry `[Read, Bash]`, implementer without
  `MultiEdit`) and gives planner `Glob`, while the v2 roster declares the wider
  sets these files carry. So the plugin's tool grants follow **v2**, and
  `fd-agent-graph bind --check-host` will report `binding.host.tools-drift` for the
  four overlapping roles if you point it at `roster/team-v3.yaml` — that is a
  roster mismatch, not a broken agent. For a pure v3 crew, generate the
  definitions instead:

  ```bash
  cd fd-agent-graph
  PYTHONPATH=. python3 -m fd_agent_graph.cli export --manifest roster/team-v3.yaml --out ./agents-v3
  ```

  Check whichever pairing you ship: `bind --manifest <roster> --host-agents <dir>`
  must report 0 errors before publishing.
  `tools`, `disallowedTools`, `skills`, `mcpServers`, `thoughtLevel` and `color`
  are agent-frontmatter fields the host parses and enforces. **Memory authority is
  not one of them** — it is stated in the body as prose (this host's `memory` field
  selects a directory scope, not an authority), so treat that line as guidance
  rather than as an enforced constraint.
- The crew's `agent-graph-gate` skill is also published standalone under
  `skills/agent-graph-gate/`: it checks an agent collaboration graph's shape before
  dispatch — port type compatibility, effect declarations and spend guards, retry
  idempotency, and whether the merge point drops a branch's constraint.

Source of truth for the crew roster is the `fd-agent-graph` package's
`roster/finddata-coding.yaml`; `plugins/finddata-coding-crew/agents/*.md` are
generated from it (`fd-agent-graph export`), and a unit test fails if they drift.

Demo playbook: `docs/vertical-packs.md` in the paas repo (fd-craw-private).
