# Prompt 5 — The Full Autonomous Loop

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

Run `/loop stop` to cancel at any time.
