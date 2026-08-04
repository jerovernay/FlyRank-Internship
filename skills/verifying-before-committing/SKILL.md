---
name: verifying-before-committing
description: Independent re-derivation checklist that runs BEFORE committing, finalizing, or reporting a completed task. Trigger when the user says words like "done", "commit", "push", "revise", "finalize", "submit", "ship it",or  "wrap up" — OR when you are about to report a task as complete, write a commit message, or close out a multi-step problem. Do NOT run this after every prompt or mid-task — only at the finish line.
---
# Verifying before committing

Re-reading a cell is not verification — it just confirms the code executed. A confident-looking
number that comes out quickly is exactly the case that hides bugs: it "passed" a first look
because nobody looked hard. This skill is the habit of catching that yourself, before the human
has to ask.

## When to trigger
 
Run this checklist when ANY of these are true:
 
1. **Explicit trigger words** — the user says: done, commit, push, revise, finalize, submit,
   ship, wrap up, check it, review,  merge.
2. **Implicit task completion** — you are about to:
   - Write a commit message
   - Report "task complete" or summarize what was done
   - Move on to a different problem or file
   - Close out a notebook, script, or function you were building
3. **User asks to review** — any request to double-check, verify, or validate results.
## When NOT to trigger
 
- Mid-task, between incremental steps
- After simple questions or explanations
- During brainstorming or planning
- After minor edits (typo fixes, comment changes, import reordering)


## The rule

**Run this before every commit, not only when asked.** If a metric, claim, or table is about to
be written into a notebook, a paper section, or a commit message, it gets one independent
re-derivation first. "Independent" means a second path to the same number — not staring at the
first path again.

## How to re-derive independently

Pick whichever applies:
- **Standalone script from source.** Recompute the number from the raw/source table in a fresh
  script or cell that doesn't import the pipeline that produced it. If both paths agree, that's
  real evidence — not two reads of the same bug.
- **Cross-check against a second source.** A different table, a different aggregation level, or
  a sanity bound (e.g. a rate must be in [0, 1], a count must not exceed the population).
- **Assert against exported artifacts.** If a claim says "sealed holdout" or "this file has N
  rows," write the assert and run it — don't eyeball it.
- **Hand-computation on a small slice.** Pull 5-10 rows, compute the metric by hand or in a
  throwaway cell, compare to the pipeline's output for those same rows.

## What to scrutinize hardest

- Anything that "worked on the first try," especially formulas with more than one probability,
  rate, or ratio in them (joint vs. conditional is a classic silent bug).
- Chart/table ordering, axis direction, sign of a lift or delta — easy to get backwards without
  erroring.
- Any number that will be quoted in prose (the paper, a commit message, a claim) — text doesn't
  get re-executed, so it's the last line of defense before the number goes stale in someone's head.
- Population/filter definitions that changed recently — rerun the count, don't assume it's stable.

## The checklist

- [ ] Second, independent path to the number exists (script, cross-source, hand-calc, or assert)
- [ ] Both paths agree — and if they don't, the discrepancy is resolved, not averaged away
- [ ] Sanity bounds checked (ranges, counts, monotonicity where expected)
- [ ] Ordering/direction of anything visual (chart, ranked table) checked against the claim it supports
- [ ] Notebook re-run top to bottom after the fix, not just the one cell

## How to verify you're using this skill right

- You can name the second path you used for each number that shipped this session.
- At least once per non-trivial task, the first pass and the recheck actually disagreed — if they
  never disagree, the recheck probably isn't independent enough.
- Nothing gets committed on the strength of "it ran without an error."
