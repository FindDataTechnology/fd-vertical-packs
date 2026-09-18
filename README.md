# fd-vertical-packs

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

Demo playbook: `docs/vertical-packs.md` in the paas repo (fd-craw-private).
