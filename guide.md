# Self-Learning PRD Loop

A background agent that watches how you write a PRD, detects patterns in how it evolves, and compounds targeted improvements into your quality checklist over time — automatically, on a 5-minute heartbeat.

The loop does not rewrite your PRD. It acts as a silent observer: it sees what you changed, infers what you are prioritizing, and suggests what you are missing. You stay in control; the agent compounds your quality over time.

---

## What You Get

1. A Claude Code workspace with a live PRD and a 20-point quality checklist
2. A baseline snapshot file that resets on every approved cycle
3. A background cron task that diffs, analyzes, and logs changes every 5 minutes
4. An email notification with an Approve button that writes approved items back to your checklist
5. A `/loop` command that orchestrates all of the above into one autonomous watcher

---

## Why This Works

A PRD is never written in one sitting. It is built iteratively — placeholders filled in, sections restructured, priorities reordered. That evolution contains a signal: **how you edit tells you what you are thinking about and what you are avoiding**.

The loop captures that signal by comparing each version of the PRD against the last known baseline. Each 5-minute cycle:

1. Reads the latest state of your PRD from disk
2. Diffs it against the frozen baseline
3. Infers patterns — which sections changed, what types of edits, placeholder fill rate
4. Generates targeted checklist improvements conditioned on those patterns
5. Freezes the new state as the next baseline

The feedback signal is not test pass/fail — it is **PRD quality patterns**. The closer your PRD is to the 20-point checklist standard, the fewer new suggestions the agent generates. When suggestions stop, your PRD is done.

---

## Prerequisites

- **Claude Pro subscription** — required to run scheduled tasks (`/cron`) and the `/loop` skill inside Claude Code
- **`prd-checklist.md`** — the 20-point PRD quality standard that drives the loop. A starter version is included in this repo at `/checklist/prd-checklist.md`

---

## Setup

### Step 1 — Create Your Workspace Folder

1. Open **Claude Code** (desktop app or run `claude` in your terminal)
2. Create a new folder on your machine called `prd-loop` (or any name you prefer)
3. Place `prd-checklist.md` inside it

Your folder before running any prompts:

```
prd-loop/
└── prd-checklist.md
```

### Step 2 — Add The Folder to Claude Code

1. In Claude Code, click **Add** (or `File → Open Folder`)
2. Select your `prd-loop` folder
3. Claude Code now has direct read/write access to all files in this folder

---

## The Five Prompts

Run these in order. Each prompt builds on the previous one.

---

### Prompt 1 — Generate the PRD

```
Create a Product Requirements Document (PRD) for [your product or feature] based on my prd-checklist,
ensuring that all checklist items are fully addressed in the document, and save the generated PRD
in the same workspace folder with name prd.md.
```

**What happens:** Claude reads `prd-checklist.md`, maps each checklist item to a PRD section, and writes a structured first draft. Sections requiring real business context contain `[Fill in]` placeholders — this is intentional.

**What you get:** `prd.md` — a full PRD skeleton with every checklist section present.

**After this prompt:** Fill in a few `[Fill in]` placeholders — your product name, a real user pain point, one success metric. This gives the loop real signal on its first cycle.

---

### Prompt 2 — Freeze the Baseline

```
Please create a file named dummy_prd.md as an exact copy of prd.md. This file will act as the
baseline snapshot for tracking changes and should never be edited manually.
```

**What happens:** Claude creates `dummy_prd.md` as a byte-for-byte copy of `prd.md` at this moment.

**What you get:** `dummy_prd.md` — the frozen reference point.

**Why two files:** The loop answers "what changed since the last cycle?" by diffing `prd.md` (live) against `dummy_prd.md` (frozen). Never edit `dummy_prd.md` by hand — the agent owns it.

**After this prompt:** Make a few more edits to `prd.md` before running Prompt 3.

---

### Prompt 3 — Start the Background Change Tracker

```
Create a scheduled task called prd-change-tracker that runs every 5 minutes with the following prompt:

You are the PRD Loop Tracker. On each run, compare prd.md with dummy_prd.md and identify patterns
in how the PRD has changed. If changes are found, for each change, record the section it belongs to,
the type of change (Addition, Deletion, or Modification), and include both the before text
(from dummy_prd.md) and after text (from prd.md). Then generate a Pattern Analysis explaining
what types of changes were made (such as filling placeholders, restructuring, or adding detail),
which sections were modified, what this indicates about the PM's current focus, and the percentage
of [Fill in] placeholders that have been completed versus those remaining. After that, generate
5–8 specific and actionable checklist items tailored to the detected patterns. Save all detected
changes, pattern analysis, and generated checklist items in a file named prd-pattern-log.md.
Finally, update dummy_prd.md to match the latest version of prd.md so it remains the baseline
for the next run.
```

**What happens:** Claude registers a background task that wakes every 5 minutes, diffs the files, analyzes patterns, logs findings, and resets the baseline.

**What you get:**
- A live background task in Claude Code
- `prd-pattern-log.md` — append-only log of every diff cycle with timestamps, change records, pattern analysis, and checklist items
- `dummy_prd.md` auto-updated at the end of each cycle

---

### Prompt 4 — Email Notification With Approval

```
Read the latest entry in prd-pattern-log.md. Compose and send an email to [your email address]
summarizing the patterns detected and the suggested checklist items. Include an Approve button
that, when clicked, appends the approved items to prd-checklist.md.
```

**Setup required:** Connect Gmail via the MCP connector in Claude Code settings before running this prompt.

**What happens:** Claude reads the latest log entry, composes a structured email, and sends it via the Gmail MCP connector. The Approve button writes approved items directly back to your checklist.

**What you get:**
- An email summarizing what the agent observed and why it is suggesting each item
- A one-click approval flow — no copy-paste required

**Why the approval step matters:** The loop does not blindly rewrite your checklist. You decide what belongs. Approved items compound into your checklist. Items you do not approve are discarded.

---

### Prompt 5 — The Full Autonomous Loop

```
/loop 5m

① Detect changes
Compare prd.md against dummy_prd.md. If no changes are found, stop and wait for the next interval.
If changes are found, identify each one by: section name, change type (add / delete / modify),
and a before-vs-after diff.

② Analyze patterns
Examine the changes to: identify how the PRD is evolving, infer what the PM is currently
prioritizing, and calculate the percentage of placeholders that have been completed.

③ Suggest improvements
Based on the patterns found, generate 5–8 new checklist improvements.

④ Log and notify
Append a timestamped entry to prd-pattern-log.md containing all of the above. Then send an email
including: patterns identified, why those patterns matter, suggested checklist updates, and an
Approve button.

⑤ Handle approval
If the Approve button is clicked: trigger the configured hook or create one and update
prd-checklist.md with the approved items.

⑥ Sync baseline
Update dummy_prd.md to match the current state of the PRD, so the next loop has a clean
baseline to diff against.
```

**What this is:** The `/loop` skill chains Prompts 3 and 4 into a single autonomous agent running on a 5-minute heartbeat indefinitely.

---

## How `/loop` Works

When you run `/loop 5m`, Claude Code:

1. Registers a recurring schedule (every 5 minutes, configurable)
2. On each tick, invokes the prompt as a fresh agent context
3. The agent reads current state from disk — no memory of prior runs needed, the files are the memory
4. Executes steps ① through ⑥ in order
5. Exits cleanly and waits for the next tick

| Step | What it does | Why it matters |
|---|---|---|
| ① Detect changes | Diff `prd.md` vs `dummy_prd.md` | No changes = no-op. Saves tokens on idle cycles. |
| ② Analyze patterns | Classify change types, measure placeholder fill rate | Turns raw diffs into insight about PM editing behavior |
| ③ Suggest improvements | Generate 5–8 checklist items conditioned on patterns | Targeted suggestions beat generic checklists |
| ④ Log and notify | Append to `prd-pattern-log.md`, send email | Durable audit trail + human review trigger |
| ⑤ Handle approval | Webhook → append to `prd-checklist.md` | Human-in-the-loop gate; agent never self-authorizes |
| ⑥ Sync baseline | `dummy_prd.md` = current `prd.md` | Resets diff surface; next cycle starts clean |

**Stop conditions:**
- Run `/loop stop` to cancel
- PRD reaches 100% placeholder fill rate and no new gaps are detected
- Claude Code workspace is closed

---

## File State After 10 Cycles

```
prd-loop/
├── prd-checklist.md      ← original 20 points + approved additions from cycles
├── prd.md                ← your live PRD, increasingly complete
├── dummy_prd.md          ← auto-updated baseline; always = state of prd.md at last cycle end
└── prd-pattern-log.md    ← append-only log; 10 timestamped entries, one per cycle
```

By cycle 10, `prd-checklist.md` has grown from 20 items to 28–30 — all tailored to how *you* write PRDs, not a generic template.

---

## Files In This Repo

| File | Purpose |
|---|---|
| `prompts/prompt-1-generate-prd.md` | Generate the initial PRD from your checklist |
| `prompts/prompt-2-baseline.md` | Freeze the baseline snapshot |
| `prompts/prompt-3-cron-tracker.md` | Start the background change tracker |
| `prompts/prompt-4-email-notify.md` | Email notification with approval gate |
| `prompts/prompt-5-loop.md` | Full autonomous loop command |
| `checklist/prd-checklist.md` | 20-point PRD quality standard |

---

## Key Principles

- **Edit history is signal.** Which sections you touch first and which you avoid tells the agent what you're prioritizing and deferring.
- **Two-file baseline = clean diffs.** Live file + frozen snapshot scopes every diff to exactly one cycle.
- **Files are the memory; the agent is stateless.** State lives on disk. Each tick reads it fresh — no context drift.
- **Human-in-the-loop is an architectural decision.** The Approve button prevents the agent from self-modifying your checklist.
- **Stop conditions are domain-specific.** This loop stops when PRD completeness is reached — not when a test passes.

---

## Built by
Prasad MK · [linkedin.com/in/prasadmk](https://linkedin.com/in/prasadmk) · prasad.mks@gmail.com
