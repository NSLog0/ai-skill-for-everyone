---
description: Work a card (ticket) in an auditable way — stop for approval before writing code
argument-hint: PROJ-123
---

Work card **$1** using the steps below. **Do not skip steps or change their order.**

Project-specific values come from the **"Card workflow"** section of the project's `CLAUDE.md`: card tracker, build/test commands, invariants, which docs are the source of truth, and the handover folder.
If that section is missing, ask the user before starting. Don't guess.

---

## Step 1 — Read the card

Fetch `$1` from the tracker named in `CLAUDE.md`, e.g. Jira, Linear or GitHub Issues, through MCP or a CLI.
If you can't fetch it, ask the user to paste the card.
Read the description, the acceptance criteria, the DoD and the linked cards in full.

## Step 2 — Open only the docs the card references

**Don't read the whole docs folder.** Open only the files and sections the card mentions.
If the card cites no docs, use the "which doc is the source of truth" table in `CLAUDE.md`.
If `CLAUDE.md` says a topic needs a specific skill, load that skill before writing code.

## Step 3 — 🛑 Stop and propose a plan before the first line of code

Propose in this format, then **wait for approval**. Never write code in the same message as the proposal.

```
## $1 — <card title>

**What I'll build**
<2-4 lines: which files, which endpoints or functions>

**Relevant invariants**
<which rules in CLAUDE.md apply, and what needs extra care>

**Tests I'll write first**
<test case names, including cases that prove the invariants above hold>

**⚠️ What the card doesn't say**
<places I'd have to guess, conflicts between the card and the docs, anything business or legal must decide.
 If there are none, write "none". Never skip this silently.>
```

**If you hit something that business, legal or the work owner must decide**, or a doc says "pending", "undecided" or ⚠️, **stop here**. Don't pick an option yourself and carry on.

## Step 4 — Branch, then write tests before code

**Create a branch named after the card.** `/audit` reviews `git diff <main>...HEAD`.
If you work on main directly, the auditor can't tell which changes belong to this card.

```bash
git switch -c $1        # start from a clean main
```

- Write the tests proposed in step 3. See them fail first, then make them pass.
- Keep tests proportional to the risk. Test the behaviour the card changes and the edge cases that genuinely differ. Don't write cases that repeat the same meaning.
- **Don't widen the scope.** If you find a bug outside the card, note it for the step 5 report. Don't fix it.
- If you hit complexity step 3 didn't foresee, **go back to step 3**. Don't decide on your own and carry on.

## Step 5 — Write the handover report as a file

Write it to the handover folder named in `CLAUDE.md` (default `docs/handover/$1.md`), using the `HANDOVER_TEMPLATE.md` template.

**Not a chat message.** The auditor reads this file, not the chat.

- **Every DoD item needs a command the auditor can copy, run, and get the same result.** "It passes" is not evidence. If a command can't prove it, cite `file:line`.
- **Never mark ✅ and then explain it's "almost done".** Incomplete is ❌. Unverifiable is ⚠️, with the reason.
  A DoD item with two parts is ✅ only when both pass.
- If any test fails, show the real output.
- In the **"if this choice is wrong, how would we know"** column, if the answer is "nothing would catch it", say so plainly.
- **Deviations from the step 3 plan:** go through the approved plan item by item and say what changed. A diff can't show what you didn't do.
- **Open sub-items in the source docs:** read the whole section the card cites, not just the quoted part.
- If a section has no content, write **"none"**. Never delete a section.

⚠️ This report is a **claim waiting to be proven**. The auditor will re-run every item.

## Step 6 — Commit for review

Commit the work **together with the report file** to the card's branch. The commit message says what changed and why.
Then tell the user it is ready for `/audit $1`.

---

## Not yet (unless told to)

- Don't push, merge or open a PR
- Don't change main directly
- Don't change the card's status
- Don't bypass git hooks
