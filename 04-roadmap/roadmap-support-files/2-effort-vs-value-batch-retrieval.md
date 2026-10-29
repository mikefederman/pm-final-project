# Batch Retrieval Feature Proposals: Effort vs. Value Scoring (Simulated)

*All research data in this document is simulated.*

**Constraints:** Team of 2 engineers + 1 designer on a 3-week sprint, which is about 6 engineer-weeks and 3 designer-weeks (assumption: arithmetic with no buffer). Engineer-week figures are estimates, not team-validated. The core batch endpoint (1,000-part cap, 1,500 ms gate) is treated as in scope separately and has no estimate here, so the capacity figures below exclude it. The feature set is a backend capability for a pilot with a select team of engineers vetted for security.

## Effort vs. Value: Ranked by Quadrant

Within each quadrant, features are ordered by Value (high to low), then Effort (low to high). Scores are unchanged from the original scoring except B5, which is rescored from Value 5 to Value 3 because it is now a workflow-side capability (see Score Change). The Disposition column records the Module 4 placement.

| Quadrant | # | Feature | Value | Effort | Est. Eng-Weeks | Fits 3-Week Sprint | Disposition | Rationale |
|---|---|---|---|---|---|---|---|---|
| Quick Win | B2 | Partial-Success Batch Responses | 5 | 2 | 1.5 | Yes | Pilot core | A BOM-wide request will almost always contain a bad part number, and an all-or-nothing failure pushes the engineer back to loops or dumps (31% already abandon after error/timeout). |
| Quick Win | B3 | Batch-Aware Quota Counting | 4 | 2 | 1 | Yes | Pilot core | Without it, a 100+ part review still hits 429s (12% vs. 5% six months ago) and the engineer abandons the sanctioned path. Quota values stay with APIM. |
| Quick Win | B1 | Slim Payload Mode | 4 | 2 | 1 | Yes | Pilot core (absorbed) | The release returns the payload required to automate a lookup, so workflow builders do not have to strip UI data from every response. It keeps 1,000-part responses inside the 1,500 ms guardrail and lowers token cost as a benefit. It supports BOM-scale completion indirectly. |
| Major Project | None | None | None | None | None | None | None | No feature scored high value with high effort. |
| Fill-In | B7 | First-Batch Quickstart Kit | 3 | 1 | 0.5 | Yes, mostly designer | Deferred (designer work) | It addresses the Mo. 0 to 1 drop, but that onboarding link is an inference and Mo. 1 retention is not the Primary Metric. |
| Fill-In | B5 | BOM File Ingestion | 3 | 3 | 2.5 | Yes, with designer | Deferred (workflow-side) | The batch endpoint already accepts part numbers, and converting a BOM file into a batch is a step the workflow can handle. Value is rescored from 5 to 3 for that reason (unvalidated). |
| Fill-In | B4 | Secure Bulk Export | 3 | 4 | 4 | No | Deferred | Scored 3 and 4 (a Time Sinker) and reclassified to Fill-In by the PM. It targets the CSV-dump behavior (33% suspected workaround), but a download is not a BOM-scale API session, so it does not move the Primary Metric. Security work is shared with InfoSec. |
| Fill-In | B6 | Bulk Access Audit Dashboard | 2 | 3 | 2.5 | Marginal | Deferred (assumed for pilot) | It measures anomalous events (412 per month) but changes no engineer behavior. Monitoring already exists for the legacy UI, and logging and reporting are assumed for the pilot. |
| Time Sinker | B8 | Agent Connector SDK | 2 | 4 | 5+ | No | Cut | Its origin is Pipeline Automators (22% of base, opportunity index 220 vs. 902 for Equipment Review Engineers), and it would consume most of the sprint. |
| Time Sinker | B9 | Async Batch Jobs with Callback | 2 | 4 | 4 | No | Cut | Origin is Engineering preference, the core use case is a synchronous 1,000-part call under 1,500 ms, and background processing sits outside that guardrail. |
| Time Sinker | B10 | Part Alternates Suggestions | 1 | 5 | 6+ | No | Cut | It has no tie to the moment of misery or any M2/M3 evidence, and it needs new substitute-part data and logic that exceed the whole sprint. |

## Quadrant Summary

| Quadrant | Features | Est. Eng-Weeks |
|---|---|---|
| Quick Win | B2, B3, B1 | About 3.5 |
| Major Project | None | None |
| Fill-In | B7, B5, B4, B6 | About 9.5 |
| Time Sinker | B8, B9, B10 | About 15 or more |

## Pilot Core Capacity

| Item | Est. Eng-Weeks |
|---|---|
| B2 Partial-Success Batch Responses | 1.5 |
| B3 Batch-Aware Quota Counting | 1 |
| B1 Slim Payload Mode (absorbed) | 1 |
| **Pilot core features** | **3.5 of about 6** |
| Core batch endpoint | Not estimated |
| Pilot measurement (MoSCoW M6, unscored) | Not estimated |

The remaining capacity is not a confirmed buffer, because the core endpoint and the pilot measurement have no estimate, and the endpoint carries known risks (N+1 query degradation and the 1,500 ms gate). The Quick Win quadrant and the pilot core now contain the same three features.

## Designer Role

| Item | Detail |
|---|---|
| Original designer work | B5 and B7 |
| Revised | B5 is workflow-side, so B7 is the only scored designer item. |
| Proposed designer work (assumption) | Developer documentation, the B7 quickstart kit, and the demo prototype. The research does not define the role. |

## Score Change

| Feature | Original | Revised | Reason |
|---|---|---|---|
| B5 BOM File Ingestion | Value 5, Effort 3 (Quick Win) | Value 3, Effort 3 (Fill-In) | The original value assumed an intake step for the engineer. As a workflow-side capability, it is not delivered by the backend feature, and the batch endpoint already accepts part numbers. A feature deferred from the pilot core is no longer labeled a Quick Win. The new value is a proposal. |

## Open Items

| # | Item |
|---|---|
| 1 | Core batch endpoint effort: estimate separately or add a placeholder to the 6 engineer-weeks |
| 2 | Required payload field set for B1 |
| 3 | Confirm the B5 rescore (Value 5 to 3), which is a proposal and unvalidated |
| 4 | Designer role confirmation |
| 5 | Pilot measurement (M6) effort: estimate it, since it is required to read the primary metric. It is a measurement requirement, not a feature proposal, so it is not scored for value. |
