# Self-Learning PRD Loop

An agentic feedback mechanism that continuously ingests customer feedback, prototype validations, and PRD reviews — compounding that signal into an improving knowledge base for authoring product requirements.

---

## The Problem

Most PM quality frameworks are static — a checklist written once, applied the same way forever, regardless of how the PM or the product evolves. They don't learn from how you write, what you prioritize, or what you consistently miss.

A PRD's edit history contains more signal than the PRD itself — which sections get tackled first, which get avoided, how priorities shift across drafts. This system captures that signal and compounds it into a personal quality standard over time.

---

## How It Works

![Self-Learning Loop Architecture](https://raw.githubusercontent.com/prasadmks/self-learning-prd-loop/main/image/self-learning-loop-architecture.png)

A background agent runs on a 5-minute heartbeat. Each cycle detects how the PRD has evolved, infers what the PM is prioritizing, generates targeted checklist improvements, and emails them for approval. Approved items compound into the checklist. The agent suggests — the PM decides.

```
① Detect changes      → diff live PRD against frozen baseline
② Classify patterns   → infer PM priorities from edit behavior
③ Generate items      → 5–8 targeted checklist improvements
④ Email for approval  → PM reviews digest, approves or discards
⑤ Write back          → approved items added to checklist
⑥ Reset baseline      → next cycle starts clean
```

---

## The Design Decision

The agent is stateless. The workspace is stateful. Every cycle reads fresh from disk — no context drift, no compaction artifacts across runs.

| | Static checklist | Self-Learning Loop |
|---|---|---|
| Adapts to your writing style | No | Yes |
| Learns from feedback signals | No | Yes |
| Improves over time | No | Yes |
| Human controls every change | N/A | Always |
| Stop condition | Never | PRD completeness reached |

---

## What's In This Repo

```
self-learning-prd-loop/
├── README.md
├── guide.md                         ← full architecture walkthrough
├── prompts/
│   ├── prompt-1-generate-prd.md     ← generate PRD from checklist
│   ├── prompt-2-baseline.md         ← freeze baseline snapshot
│   ├── prompt-3-cron-tracker.md     ← start background change tracker
│   ├── prompt-4-email-notify.md     ← email digest with approval gate
│   └── prompt-5-loop.md             ← full autonomous loop command
├── checklist/
│   └── prd-checklist.md             ← 20-point PRD quality standard
└── images/
    └── self-learning-loop-architecture.png
```

---

## Built by
Prasad MK · [linkedin.com/in/prasadmk](https://linkedin.com/in/prasadmk) · prasad.mks@gmail.com
