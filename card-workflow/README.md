# card-workflow — auditable card-driven work with Claude Code

Three [Claude Code](https://claude.com/claude-code) commands that separate the agent that does the work from the people who check it. Each step can run in a different session.

| Command | Who runs it | What it does |
|---|---|---|
| `/card PROJ-123` | the coding agent | Reads the card and proposes a plan, then **stops for approval before touching code**. Writes tests first, and hands over a report containing commands that prove the work |
| `/decide PROJ-123` | the work owner (pastes the plan from `/card` after the command) | Fact-checks the plan's claims and looks for what the agent didn't ask. Produces a reply ready to send back to the agent |
| `/audit PROJ-123` | the work owner (after the work is done) | Checks the real work against the DoD. **It doesn't trust the report and re-runs every check itself.** It never fixes code |

## Why it works

- **There is a stop before any code is written.** You agree on what to build while it's still cheap to change, not after 500 lines exist.
- **Reports must be runnable commands.** "It passes" is not evidence.
- **The auditor may not fix code.** If the auditor fixes things, nobody learns what the agent can actually do.
- **It forces the things that usually get buried into the open.** For example, the report must say when nothing would catch a wrong choice, where the agent deviated from the plan, and which open items remain in the docs.

## Install

**Just for you, in every project:**
```bash
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```

**For a whole team, in one project:**
```bash
mkdir -p .claude/commands docs/handover
cp commands/*.md .claude/commands/
cp templates/HANDOVER_TEMPLATE.md docs/handover/TEMPLATE.md
git add .claude/commands docs/handover && git commit -m "add card workflow commands"
```

## Configure your project (the most important step)

These commands can only check what your project's `CLAUDE.md` tells them about.
Copy [`templates/CLAUDE-card-workflow-section.md`](templates/CLAUDE-card-workflow-section.md) into `CLAUDE.md` and fill in:

1. **Card tracker.** For Jira, connect the Atlassian MCP in Claude Code. GitHub Issues works through `gh`. With no tracker, paste the card in yourself.
2. **Build, test and typecheck commands.**
3. **Invariants that must never be broken**, each with how to check it. This is what lets `/audit` catch real mistakes.
4. **Which document is the source of truth for what.** This keeps agents away from stale docs.
5. **Open decisions.** The agent stops and asks about these instead of guessing.

## Workflow

```
/card PROJ-123      → agent proposes a plan, then stops
/decide PROJ-123    → (another session) paste the plan → get a reply for the agent
                     → agent writes tests + code + report, commits to a branch
/audit PROJ-123     → (another session) verify for real → pass / needs fixes
```

Run `/decide` and `/audit` in separate sessions, so the reviewer doesn't share the implementer's framing.

## Notes

- The command files and templates are written in Thai. Claude follows them fine and replies in your language. Translate them if your team needs to edit them.
- Before using this with company code or data, check your workplace's AI usage policy.
- `/audit` runs tests and migrations. Point it at a test database or environment only.
