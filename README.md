# Self-Learning PRD Loop

A background agent that watches how you write a PRD, detects patterns in how it evolves, and compounds targeted improvements into your quality checklist over time — on a 5-minute heartbeat.

The agent suggests. You decide. Every approved item compounds into a personal PRD quality standard tailored to how you actually write — not a generic template.

---

## How It Works

```
Detect changes → Analyze patterns → Suggest improvements → Email for approval → Write back → Reset baseline
```

| Step | What happens |
|---|---|
| ① Detect | Diffs live `prd.md` against frozen `dummy_prd.md` |
| ② Analyze | Classifies change types, measures placeholder fill rate |
| ③ Suggest | Generates 5–8 checklist items conditioned on the patterns |
| ④ Notify | Emails a digest with an Approve button |
| ⑤ Approve | Approved items written back to `prd-checklist.md` |
| ⑥ Sync | Baseline resets — next cycle starts clean |

---

## Prerequisites

- [Claude Code](https://claude.ai/code) installed
- Claude Pro subscription (required for `/cron` and `/loop`)
- Gmail connected via MCP connector in Claude Code settings (for Prompt 4)

---

## Quick Start

1. Clone this repo
2. Copy `checklist/prd-checklist.md` into your working folder
3. Open the folder in Claude Code
4. Run the prompts in `/prompts/` in order — 1 through 5

Full walkthrough in [`guide.md`](guide.md)

---

## Repo Structure

```
self-learning-prd-loop/
├── README.md
├── guide.md                        ← full walkthrough
├── prompts/
│   ├── prompt-1-generate-prd.md
│   ├── prompt-2-baseline.md
│   ├── prompt-3-cron-tracker.md
│   ├── prompt-4-email-notify.md
│   └── prompt-5-loop.md
└── checklist/
    └── prd-checklist.md
```

---

## Key Principles

- **Files are the memory — the agent is stateless.** Each tick reads fresh from disk. No context drift across cycles.
- **Human-in-the-loop is an architectural decision.** The agent never self-modifies your checklist. You approve every change.
- **Stop conditions are domain-specific.** The loop stops when PRD completeness is reached — not when a test passes.
- **Pattern-conditioned suggestions beat generic checklists.** Items generated from your actual edits are more actionable than a static template.

---

## Built by
Prasad MK · [linkedin.com/in/prasadmk](https://linkedin.com/in/prasadmk) · prasad.mks@gmail.com
