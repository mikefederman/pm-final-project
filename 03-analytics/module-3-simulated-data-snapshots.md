# Module 3 Simulated Data Snapshots

All figures in this document are simulated. They are assumptions, designed to be consistent with the M2 research, not measured data.

**Scenario framing:** The API's customers are internal: the company's own engineers, teams, and the automated workflows they run. Success is measured in productivity, risk reduction, and cost avoided, not revenue. Where the course template uses consumer terms (churn, LTV), this document relabels them for an internal platform.

**Current and projected data:** Snapshot 1 and the Single-Lookup cohorts in Snapshot 2 describe the current state of the legacy UI. Values labeled "Projected" describe the planned Batch Pilot, which has not yet run. No pilot results exist.

## Contents

1. Snapshot 1: Engagement & Funnel Health
2. Snapshot 2: Engineer Retention Cohort Heatmap
3. Snapshot 3: User Segment Performance


---

## Snapshot 1 Of 3: Engagement & Funnel Health (Last 90 Days)
### Conversion Funnel

Percent of authenticated legacy UI sessions that reach each stage.

| Funnel Stage | % Of Sessions |
|---|---|
| Authenticated Session Start | 100% |
| Part Lookup Initiated | 83% |
| Part Record Retrieved | 69% |
| 10+ Lookups In Session | 38% |
| BOM-Scale Session (100+ Lookups) | 7% |

### Core Engagement Metrics

| Metric | Current | 6 Months Ago | Change |
|---|---|---|---|
| Daily Active Engineers | 8,420 | 8,150 | ▲ 3% |
| Avg Lookups Per Session | 17 | 24 | ▼ 29% |
| BOM-Scale Sessions (100+ Lookups) | 7% | 15% | ▼ 8 pts |
| Sessions Abandoned After Error Or Timeout | 31% | 18% | ▲ 13 pts |
| Requests Rate-Limited (HTTP 429) | 12% | 5% | ▲ 7 pts |
| Avg Single-Lookup Latency (p95) | 620 ms | 410 ms | ▲ 51% |
| Avg Tokens Per Lookup Response | 2,400 | 1,900 | ▲ 26% |
| Anomalous Bulk-Access Events (Monthly) | 412 | N/A | New Monitor |

**Critical signal:** Only 7% of sessions reach BOM scale, down from 15% six months ago. The top of the funnel is holding, with active engineers up 3%. The problem is depth: engineers start lookups but do not complete BOM-wide reviews through the sanctioned path.

### How The Simulated Data Links To M2 Research

| Metric | M2 Research Link |
|---|---|
| BOM-Scale Sessions, Avg Lookups Per Session | Manual looping exhaustion |
| Rate-Limited Requests | API rate limiting |
| Avg Tokens Per Lookup Response | Payload bloat and token waste, uncompressed payload latency |
| Sessions Abandoned After Error Or Timeout | Brittle error recovery |
| Single-Lookup Latency (p95) | N+1 queries, thread contention |
| Anomalous Bulk-Access Events | Shadow workarounds (scraping data from the legacy UI, CSV dumps) |

### Assumptions To Note

- **Funnel Stages:** Adapted from the course's streaming funnel (Homepage → Browse → Detail → Play → 30+ min) to API usage. The stage definitions and all percentages are assumptions.
- **Baseline Definition:** The 7% baseline counts authenticated legacy UI sessions with 100+ lookups. It is the comparison point for pilot sessions on the new batch path, which do not exist yet. Whether scraper sessions that run through the legacy UI are included in the 7% is not specified.
- **Anomalous Bulk-Access Events:** Stands in for the course's "new feature" row. Assumed to be a newly added monitor, so there is no six-month baseline. The research names scripts, scraping data from the legacy UI, and CSV dumps, but does not quantify them.
- **Token Metric:** The rise in tokens per lookup is tied to payload bloat, not to the lookup loops. The 2,400 value measures the current legacy payload.
- **Latency And Token Values:** The research identifies these as problems but gives no baselines, so the numbers are illustrative.


---

## Snapshot 2 Of 3: Engineer Retention Cohort Heatmap
Read horizontally to see how a month's cohort retains over time. Read vertically to compare newer cohorts against older ones. The Single-Lookup cohorts are simulated current-state data. The Batch Pilot cohorts (December onward) are **projections** of expected retention for the planned pilot, not observed results.

**Retention definition (assumption):** Percent of newly onboarded engineers who remain active users of the sanctioned retrieval path in each month after onboarding, rather than abandoning it for shadow workarounds. For Single-Lookup cohorts, the sanctioned path is single lookups in the legacy UI. For Batch Pilot cohorts, it is the new batch path.

### Cohort Retention

| Cohort | Mo. 0 | Mo. 1 | Mo. 2 | Mo. 3 | Mo. 4 | Mo. 5 | Mo. 6 |
|---|---|---|---|---|---|---|---|
| Sep · Single-Lookup | 🟩 100% | 🟩 64% | 🟢 46% | 🟨 35% | 🟧 27% | 🟧 21% | 🟥 17% |
| Oct · Single-Lookup | 🟩 100% | 🟩 62% | 🟢 45% | 🟨 33% | 🟧 25% | 🟥 19% | |
| Nov · Single-Lookup | 🟩 100% | 🟩 60% | 🟨 43% | 🟨 31% | 🟧 23% | | |
| Dec · Batch Pilot (Projected) | 🟩 100% | 🟩 72% | 🟩 60% | 🟢 51% | | | |
| Jan · Batch Pilot (Projected) | 🟩 100% | 🟩 75% | 🟩 62% | | | | |
| Feb · Batch Pilot (Projected) | 🟩 100% | 🟩 77% | | | | | |

**Legend:** 🟩 Strong (60%+) · 🟢 Healthy (45 to 59%) · 🟨 Watch (30 to 44%) · 🟧 Weak (20 to 29%) · 🟥 Leak (<20%)

**Critical signal:** Batch Pilot cohorts are projected to retain about 13 points higher at Month 1 than Single-Lookup cohorts (74.7% average versus 62.0%). But the Month 0 → Month 1 drop remains the single largest leak regardless of cohort type.

### Month 0 → Month 1 Drop By Cohort Type

| Cohort Type | Month 0 → 1 Drop | Month 1 → 2 Drop (Comparable Cohorts) |
|---|---|---|
| Single-Lookup (Sep, Oct, Nov) | 36 to 40 pts | 17 to 18 pts |
| Batch Pilot, Projected (Dec, Jan, Feb) | 23 to 28 pts | 12 to 13 pts |

### How The Simulated Data Links To M2 Research

| Pattern | M2 Research Link |
|---|---|
| Steep Mo. 0 → 1 drop in Single-Lookup cohorts | Manual looping exhaustion, API rate limiting, brittle error recovery |
| Higher projected Mo. 1 retention in Batch Pilot cohorts | Missing bulk retrieval as the root gap |
| Continued projected Mo. 0 → 1 leak in Batch Pilot cohorts | Residual friction: onboarding, payload bloat, shadow workarounds |
| Consistent with Snapshot 1 | Fewer BOM-scale sessions and more anomalous bulk-access events |

### Assumptions To Note

- **Cohort Split:** The course's "Spotlight" feature is replaced with a planned Batch Pilot for the December cohort onward. The pilot is a select team of engineers vetted for security. The projected cohorts model newly onboarded engineers who are given access to the batch path, which is an assumption used to show the expected effect of bulk retrieval.
- **Projection:** The Batch Pilot rows are projections. No pilot has run, and no observed pilot data exists.
- **Retention Definition:** Active use of the sanctioned retrieval path per month is assumed, with loss of retention read as abandonment in favor of shadow workarounds. The research gives no retention data.
- **Percentages:** All values are illustrative. They are set so the projected Month 1 gap is about 13 points and Month 0 → 1 stays the largest drop.
- **Cohort Months:** Labels mirror the course template and are not tied to calendar dates.
- **Causality:** The cohort differences are consistent with the M2 pain points but do not prove them.


---

## Snapshot 3 Of 3: User Segment Performance
Performance by user segment. Which group (Pipeline Automators, Equipment Review Engineers, or Occasional Lookup Users) offers the highest business impact? Hours saved and abandonment improvement are projections for the planned Batch Pilot.

### Segment Performance

| Segment | % Of Engineer Base | Est. Hours Saved / Engineer / Mo. (Projected, Batch) | Sessions / Wk | Primary Access Mix | Projected Abandonment To Shadow Workarounds If Batch Pilot-Exposed |
|---|---|---|---|---|---|
| Pipeline Automators | 22% | 9 hrs | 4.8× | 20% Single / 66% Looped / 14% Suspected Workaround | −10 pts vs non-exposed (low abandonment anyway) |
| Equipment Review Engineers | 41% | 16 hrs | 2.3× | 18% Single / 49% Looped / 33% Suspected Workaround | −22 pts vs non-exposed ✓ |
| Occasional Lookup Users | 37% | 2 hrs | 1.1× | 78% Single / 17% Looped / 5% Suspected Workaround | −4 pts vs non-exposed |

**Critical signal:** Equipment Review Engineers (41% of the base) show the biggest projected reduction in abandonment to shadow workarounds (−22 pts) when exposed to the Batch Pilot, and the largest estimated hours saved. A third of their access is suspected workaround activity. Pipeline Automators and Occasional Lookup Users are not the primary opportunity.

### Opportunity Index

| Segment | % Of Base | Projected Abandonment Improvement (Pts) | Index (Base × Pts) |
|---|---|---|---|
| Pipeline Automators | 22 | 10 | 220 |
| Equipment Review Engineers | 41 | 22 | 902 |
| Occasional Lookup Users | 37 | 4 | 148 |

The base-weighted projected abandonment improvement across all segments is about 12.7 pts, matching the projected Month 1 retention gap in Snapshot 2.

### How The Simulated Data Links To M2 Research

| Pattern | M2 Research Link |
|---|---|
| Review Engineers: 49% looped lookups | Manual looping exhaustion |
| Review Engineers: 33% suspected workaround | Shadow workarounds (scraping data from the legacy UI, CSV dumps); persona friction |
| Review Engineers: largest projected reduction in abandonment | Persona is the equipment review engineer; the shadow workaround is a symptom of the missing bulk path |
| Automators: 66% looped lookups | API rate limiting, payload bloat and token waste |
| Occasional Users: low batch benefit | Single-lookup flow already meets their need |

### Assumptions To Note

- **Segments:** The course's Power Users, Casual Browsers, and Wanderers are replaced with three API user segments. The names, definitions, and sizes are assumptions. Only the Equipment Review Engineer is supported by the M2 persona.
- **Est. Hours Saved:** Replaces the course's LTV. It is an assumed estimate of review hours saved per engineer per month if batch retrieval replaced manual lookups. The research gives no time or dollar values.
- **Abandonment To Shadow Workarounds:** Replaces the course's churn. Internal engineers rarely leave voluntarily, so the loss is modeled as moving from the sanctioned retrieval path to scripts, scraping data from the legacy UI, or CSV dumps.
- **Primary Access Mix:** Replaces the course's content mix. "Looped" means sequential single lookups, and "Suspected Workaround" is inferred from anomalous bulk-access patterns, not confirmed. The three categories are assumed to be mutually exclusive and to sum to 100% for each segment.
- **Batch Pilot Exposure:** Uses the same planned-pilot assumption as Snapshot 2. All exposed values are projections, and no pilot has run.
- **Opportunity Index:** A simple derived measure (% of base × projected abandonment improvement points), not a course metric.
- **Causality:** The abandonment differences are consistent with the M2 pain points but do not prove them.
