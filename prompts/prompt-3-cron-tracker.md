# Prompt 3 — Start the Background Change Tracker

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
