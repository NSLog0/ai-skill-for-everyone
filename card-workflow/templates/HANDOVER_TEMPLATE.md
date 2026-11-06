# <CARD-ID> — <card title>

| | |
|---|---|
| branch | `<CARD-ID>` |
| commit | `<short sha>` |
| date | `YYYY-MM-DD` |
| done by | agent / person |

---

## 1 · DoD

Copy the text from the card **word for word**. The auditor will compare it with the card character by character.

| # | Text from the card (word for word) | Claimed | Proven by | Actual result |
|---|---|---|---|---|
| 1 | | ✅ / ❌ / ⚠️ | `<command you can copy and run>` | `<real output, only the deciding lines>` |

`✅` proven · `❌` incomplete · `⚠️` can't be verified in this card. Say why, and when it can be verified.

---

## 2 · Decisions I made (the card didn't say)

| Topic | What I chose | Why | If this choice is wrong, how would we know |
|---|---|---|---|
| | | | `<command/test that would catch it>` or **"nothing would catch it"** |

> "Nothing would catch it" is an acceptable answer. It's the thing the auditor most needs to know. Don't bury it.

### 2.1 · Deviations from the plan proposed in step 3

| Planned to do | What I actually did | Why it changed |
|---|---|---|
| | | |

> A diff shows only what was done, not what was promised and skipped. If you did everything, write **"Followed the plan fully, no deviations"**.

---

## 3 · Files touched

| File | New/changed | What it does |
|---|---|---|
| | | |

---

## 4 · Commands that actually produced these results

The auditor copies this whole block, runs it, and must get the same results. Include any setup needed first.

```bash
# <command>                    # <result>
```

---

## 5 · Found along the way but not fixed

Bugs out of scope · conflicting docs · things that should be new cards. **If there are none, write "none".**

---

## 6 · Still unanswered / needs the work owner's decision

**If there are none, write "none".**

### 6.1 · Do the source docs have open sub-items?

Read the whole section the card cites. A decided section may still contain open sub-items.
Look for "not discussed yet" · "pending" · "must decide together" · `⚠️`

| Doc:section | Open sub-item | Affects this card? |
|---|---|---|
| | | No / Yes but not done because… |

> If none are found, write **"Checked, no open sub-items"** and list which docs you opened.
