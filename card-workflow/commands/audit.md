---
description: Audit an agent's finished work against the card's DoD and the project's invariants — don't trust the report, run everything yourself
argument-hint: PROJ-123 [--dual]
---

Audit the work on card **$1**. You are **the auditor, not the fixer**.

Project-specific values (build/test/typecheck commands, invariants, high-risk paths, handover folder) come from the **"Card workflow"** section of `CLAUDE.md`.

## Step 0 — Assess the risk first

Look at `git diff <main>...HEAD --stat` and `git log --oneline <main>..HEAD`, then check:

| Criterion | How to check |
|---|---|
| Touches an invariant in `CLAUDE.md` directly | Changed files are in a high-risk path listed in `CLAUDE.md` |
| Touches auth, personal data, crypto or money | Files in those areas, or a new field that stores secrets |
| Changes the schema or a migration of an important table | The schema diff |
| First card to set a new pattern other cards will follow | Creates a new feature-level folder for the first time |

**If at least 1 criterion matches**, tell the user it's high risk and why, recommend a dual audit, and ask which mode they want:
- **`--dual` (subagent)**: faster and cheaper, but **not truly independent**, because the subagent inherits this session's framing through its prompt. Use it as a second pass for another angle.
- **Open a second session yourself**: the user opens a new terminal and runs `/audit $1` without sharing this session's results, then compares the two. Truly independent. Best for work that touches money, auth or personal data.

No criterion matches → go straight to step 1 without asking.

### If running `--dual`
Do steps 1-5 here as the main audit. The core technique is the **mutation test**: revert the changed code, re-run the tests, and see whether they really catch it.
At the same time, spawn 1 subagent:
> "Audit card $1 from the source-docs angle. Read the card word for word, and every doc it references in full sections. Find anything the card requires that the handover report doesn't mention or covers incompletely, especially where two docs conflict. Report as bullets with file:line."

**Verify every subagent finding yourself** before putting it in the report.

<EXTREMELY-IMPORTANT>
**Don't fix code** unless the user separately asks you to after reading the report. If you fix it yourself, nobody learns what the agent can actually do.
Found a bug → **write it in the report**, don't fix it.

**Don't trust the agent's report.** "It passes" is not evidence. Every item needs evidence that **you ran yourself** or **read from the file yourself**.
</EXTREMELY-IMPORTANT>

---

## Step 1 — Gather what to audit

0. Read the **handover report** (default `docs/handover/$1.md`). It is a **claim**, not evidence.
   - **No file = not ready for audit.** Tell the user and stop.
   - Entries in the "decisions I made" table that say "nothing would catch it" are where you audit hardest.
   - The "deviations from the plan" section: a diff can't show this. Ask whether the reasons hold up, and whether the change weakens the evidence.
   - The "open sub-items" section: use it as a starting point, but always reopen the docs yourself.
   - Every ✅ must match its evidence. A two-part DoD item must pass both parts.
   - Note every number the agent claims (test counts, files, tables) to compare in step 2.
1. Fetch the card. **Copy the DoD word for word** and compare it with the DoD in the report (a mismatch means it was copied wrong or the card was edited later).
2. Look at the diff: `git log --oneline <main>..HEAD` and `git diff <main>...HEAD --stat`.
3. Open the docs the card references **from the real files**.
4. Open `CLAUDE.md` for the invariants and the pinned versions.

## Step 2 — Run things, don't just read

Run the build / test / typecheck commands defined in `CLAUDE.md` and **keep the real output**.
If the card changes the schema or a migration, run the migration on an empty local DB and inspect the structure from the real DB. A correct schema file doesn't mean the migration builds it correctly.
🔴 Use only test databases and environments. Never run against production or anything that costs money.

## Step 3 — Audit in 4 layers

### Layer 1 · DoD, item by item
If a command or a file can't prove it, it **fails**. Not "probably passes".

### Layer 2 · The invariants in `CLAUDE.md`
Check every one, every time, whether or not the card mentions it. Make a table of which command checks each invariant.
Also check that nobody bumped a pinned version (look at the lockfile diff).

### Layer 3 · What the card doesn't say but the docs do
Compare the work against the **source docs** directly, not against the card. The docs may have been updated while the card is still the old version.
> Example: the card says 17 tables, but the data diagram has 18 because a table was added later and nobody updated the card.

### Layer 4 · Is the report overstated?

| Agent claims | Actually measured | Match? |
|---|---|---|
| Number of passing tests | | |
| Files touched (compare with `git diff --name-only`) | | |

If even one number doesn't match, find out why before concluding. Don't wave it off as "close enough".

Other signs:
- ✅ on items that were only partly done
- Tests that test nothing (assert constants, mock everything, no assertions)
- Added `skip` / `todo` / `only`
- A loosened type checker, or `any` / `@ts-ignore` (or the language's equivalent) added to make things pass
- Files touched outside the card's scope

## Step 4 — What the user must decide

**Stop and ask. Don't decide yourself** if:
- The work runs into something business or legal hasn't decided yet
- The docs and the card conflict and it's unclear which is right
- The agent interpreted the requirement differently from you, but theirs isn't clearly wrong

## Step 5 — Report

```
# Audit $1 — <pass ✅ / needs fixes ❌>

Mode: <single-pass / dual (subagent) / dual (two sessions)>

## DoD
| # | Text from the card (word for word) | Result | Evidence |
|---|---|---|---|

## Invariants
| Invariant | Result | Checked with |
|---|---|---|

## 🔴 Must fix before closing the card
<what's wrong · why it matters · file:line · when it breaks if left as is>

## 🟡 Observations — not blocking

## ❓ For the user to decide
<if nothing, write "none">

## Commands actually run
<list + short results, so anyone can re-check>
```

**Rules for the verdict**
- One ❌ = **not passed**. There is no "almost done".
- Unprovable = ❌
- Use ⚠️ only for "correct per the DoD, but there's something you should know". Never to hide something wrong.
- **Don't change the card's status yourself.** Leave that to the user.
