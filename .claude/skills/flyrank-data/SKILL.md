---
name: flyrank-data
description: >-
  FlyRank ML internship dataset facts + safety rules. Use whenever working with
  the starter CSV (data/raw/content_refresh_anonymized.csv) or the gated
  warehouse release: the four capstone lanes, table grains, the observable-only /
  leakage discipline, column gotchas, and public-safety output rules. Triggers:
  "which lane", "content_refresh", "warehouse release", "is this safe to
  publish", "trend_direction", "avg_position 0", "ctr", any FlyRank data task.
---

# FlyRank data (internship)

Grounded in `docs/ml-intern-dataset-and-lane-guide.md` and
`docs/data-dictionary.md`. Read those for full detail; this is the working
summary.

## Starter dataset — `data/raw/content_refresh_anonymized.csv`
- **30,000 rows × 44 cols**, **32 pseudonymized clients**. One row = one content
  item (page). All metrics = trailing 90-day window. Only row every one has is
  `impressions_90d ≥ 1` and `content_age_days ≥ 90`.
- Bigger work uses the **gated warehouse release** on Hugging Face
  (`FlyRank/internship-warehouse`, ~79M daily rows; DuckDB via notebook 03).
  Tables: `dim_clients`, `dim_content`, `fact_content_daily_performance`
  (report_date × client × content), `fact_content_query_90d`.

## The three rules that prevent 90% of mistakes
1. **Rate columns are ×100 percentages.** `ctr = 0.76` means **0.76%**. Same for
   `engagement_rate`, `scroll_rate`, `ai_traffic_pct`, `trend_pct`.
2. **`trend_direction` / `trend_pct` are the LABEL source — never features.**
   `is_declining_label = (trend_direction == "down")`. Using them (or anything
   derived from them) as a feature is the leakage notebook 02 demonstrates.
3. **IDs are for grouping only.** `content_id` / `client_id` are pseudonyms —
   use for joins and **client-holdout splits**, never as features.

## More gotchas
- `avg_position == 0` means **"no position data"**, not rank 0 (1,205 rows).
- Missingness is **systematic**, not random: keyword columns are blank along
  `content_type` lines (e.g. `feedly article` has no keyword data). A blind
  `fillna(0)` silently encodes content type. Check missingness per `content_type`.
- `scroll_rate` and `ai_traffic_pct` can exceed 100 by design.
- `ai_sessions_90d` = click-throughs from AI assistants, **not** citations or
  rankings. Sparse — never train a binary classifier on AI sessions alone.
- Product decision flags (`health_score`, `priority_score`, `action_type`,
  refresh flags) are **deliberately not in the data**. If you ever reproduce one,
  it is context/baseline only — never a discovery feature or label.

## The four capstone lanes
1. **Ranking Signal Analysis** — which safe signals associate with visibility /
   clicks / engagement / movement. EDA, correlations, simple models, effect sizes.
2. **Refresh / Content Opportunity Scoring** — rank which pages to review first
   (refresh/expand/protect/prune/monitor). Baseline score → model; precision@K.
   The whole starter pipeline (`scripts/01–05`) is this lane.
3. **Structured Content Archetype Clustering** — what performance archetypes
   exist across the inventory. Scale features → K-Means/explainable clustering →
   PCA/2D → profile → name → action-map. **Not** semantic/text clustering (metric
   & metadata only). Name clusters *after* inspecting centroids, never before.
4. **CTR / Engagement Opportunity Scoring** — which visible pages under-capture
   clicks given position. Expected-CTR-by-position, residual/gap ranking. Always
   position-adjust; apply minimum-volume filters.
   (Advanced, mentor-gated: AI Referral Opportunity; Growth/Recovery/Momentum.)

## Validation
train/test for independent rows · **client/group holdout** when pages share a
client · **time-aware split** for future prediction · **top-K review** for ranked
queues · cluster-stability for archetypes · **leakage audit every lane**.

## Public-safety output rules
- **Allowed:** pseudonymized IDs, aggregated metrics, charts from safe data,
  high-level examples, cautious observed/directional claims, generic actions.
- **Never:** client names, domains, URLs, raw queries/keywords/titles,
  credentials, "I proved Google's algorithm", "refresh caused recovery" (no
  causal claim without a design). Datasets never get committed to git (CI checks).
- Frame results as **observed / measured / directional / decision-support**.
