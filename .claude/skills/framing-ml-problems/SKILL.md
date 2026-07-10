---
name: framing-ml-problems
description: >-
  Use BEFORE writing any model or notebook for an ML task — when the goal is to
  turn a vague "predict/cluster/rank X" into a defensible problem statement.
  Forces the chain data → decision → action → cost of a wrong call, picks the
  unit of analysis, separates features/labels/leakage, and enforces careful
  (observational, decision-support) language. Triggers: "frame this", "research
  question", "what should I predict", "is this leakage", "decision/action/cost".
---

# Framing ML problems

A model is only worth building if it improves a **decision** that leads to an
**action** whose **wrong call has a cost**. Frame that chain first; the algorithm
comes last. This skill is checklist-driven — walk it in order.

## 1. The five-part question

State each of these in one sentence. If you can't, the problem isn't framed yet.

1. **Search question** — the plain-English thing you want to answer.
2. **Unit of analysis** — what does *one row / one example* mean? (a page? a
   client? a page-day? a query?) Everything downstream inherits this grain.
3. **Output** — the concrete artifact (a ranked queue? a score? a label? cluster
   profiles?). Name its shape.
4. **Action** — who does what because of the output? If no one acts differently,
   stop.
5. **Cost of a wrong recommendation** — what does a false positive cost, and what
   does a false negative cost? These are usually *asymmetric*, and they pick your
   metric (precision@K when capacity is the binding constraint, recall when
   misses are expensive, etc.).

## 2. Feature / label / leakage discipline

- **Prediction-time rule:** every feature must be knowable *before* the decision
  point. If a column is computed after (or from) the outcome, it leaks.
- **Window rule:** the feature window must not overlap the label window.
- **Proxy-label honesty:** if your label is a current-window bucket (not a future
  outcome), say so and treat it as a proxy — prefer a future-window label when
  the data allows.
- **Never feed the decision back in:** a product's own score/flag or the target's
  own derivation as a feature = circular result. The model "wins" by memorizing
  the rule, not by finding signal.
- Classify every column before modeling: *join key · observed signal · derived
  measurement · context · label/proxy · leakage-risk · excluded*.

## 3. "Why not just train a model?"

Answer explicitly. Good framings name at least one of:
- a **baseline** (a transparent rule) the model must beat, and *by what metric*;
- a **validation design** that matches the problem (client/group holdout when
  rows share a source; time-aware split for future prediction);
- a reason the task is **structure discovery or ranking under capacity**, not
  accuracy-chasing — i.e. what human judgment still decides afterward.

## 4. Careful language

Say **observed / measured / directional / decision-support**. Do **not** say
proven / caused / guaranteed. Rank "review first," don't promise "will recover."
No causal claim without an experiment or causal design.

## 5. Self-check before you build

- [ ] Unit of analysis named and consistent with the label grain.
- [ ] Decision and action named; someone acts differently because of the output.
- [ ] FP cost and FN cost stated; metric chosen to match.
- [ ] Baseline defined; validation design matches the problem.
- [ ] Leakage checked (prediction-time + window + no decision-as-feature).
- [ ] Claims are observational/decision-support, not causal.
