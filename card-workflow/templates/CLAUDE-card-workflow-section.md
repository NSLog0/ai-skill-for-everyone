<!-- Copy this section into your project's CLAUDE.md and fill it in. /card /decide /audit read their settings from here. -->

## Card workflow

| Setting | This project |
|---|---|
| Card tracker | e.g. Jira `mycompany.atlassian.net` project `PROJ` (via Atlassian MCP) / GitHub Issues `org/repo` (via `gh`) |
| Main branch | `main` |
| Build | `npm run build` |
| Test (unit) | `npm test` |
| Test (integration/e2e) | `npm run test:integration` — test database only |
| Typecheck / lint | `npm run typecheck` |
| Handover folder | `docs/handover/` (template `docs/handover/TEMPLATE.md`) |

### 🔴 Invariants that must never be broken
<!-- Rules where a violation means the work is broken, not just needing a fix. Say how to check each one. -->
1. <rule> — check with `<grep/test command>`
2. …

### High-risk paths (audit will suggest a dual review)
- `src/auth/` · `src/payments/` · `migrations/` · …

### Pinned versions — don't upgrade on your own
| Package | Version | What breaks if upgraded |
|---|---|---|

### Which doc is the source of truth for what
| Topic | File |
|---|---|

### Open decisions — don't guess
- <topic> — waiting on whom
