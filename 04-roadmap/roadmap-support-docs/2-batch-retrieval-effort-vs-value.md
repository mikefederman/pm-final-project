# Batch Retrieval Feature Proposals: Effort vs. Value Scoring (Simulated)

**Constraints:** Team of 2 engineers + 1 designer on a 3-week sprint, which is about 6 engineer-weeks and 3 designer-weeks (assumption: arithmetic with no buffer). Engineer-week figures are estimates, not team-validated. The core batch endpoint is assumed to be in scope separately, and all M3 figures are simulated.

## Effort vs. Value: Ranked by Quadrant

Within each quadrant, features are ordered by Value (high to low), then Effort (low to high).

| Quadrant | # | Feature | Value | Effort | Est. Eng-Weeks | Fits 3-Week Sprint | Rationale |
|---|---|---|---|---|---|---|---|
| Quick Win | B2 | Partial-Success Batch Responses | 5 | 2 | 1.5 | Yes | A BOM-wide request will almost always contain a bad part number, and an all-or-nothing failure pushes the engineer back to loops or dumps (31% already abandon after error/timeout). |
| Quick Win | B5 | BOM File Ingestion | 5 | 3 | 2.5 | Yes, with designer | The engineer starts from a BOM, not a part-number list, so this removes the last manual step in the moment of misery. |
| Quick Win | B3 | Batch-Aware Rate Limit Tier | 4 | 2 | 1 | Yes | Without it, a 100+ part review still hits 429s (12% vs. 5% six months ago) and the engineer abandons the sanctioned path. |
| Quick Win | B1 | Slim Payload Mode | 4 | 2 | 1 | Yes | Stripping UI elements keeps 1,000-part responses inside the 1,500 ms guardrail and cuts token cost, though it supports BOM-scale completion indirectly. |
| Major Project | None | None | None | None | None | None | No feature scored high value with high effort. |
| Fill-In | B7 | First-Batch Quickstart Kit | 3 | 1 | 0.5 | Yes, mostly designer | It addresses the Mo. 0 to 1 drop, but that onboarding link is an inference and Mo. 1 retention is not the Primary Metric. |
| Fill-In | B6 | Bulk Access Audit Dashboard | 2 | 3 | 2.5 | Marginal | It measures anomalous events (412 per month) but changes no engineer behavior, and the monitor already exists. |
| Time Sinker | B4 | Secure Bulk Export | 3 | 4 | 4 | No | It targets the CSV-dump behavior (33% suspected workaround), but a download is not a BOM-scale API session, so it does not move the Primary Metric. |
| Time Sinker | B8 | Agent Connector SDK | 2 | 4 | 5+ | No | Its origin is Pipeline Automators (22% of base), not Equipment Review Engineers, and it would consume most of the sprint. |
| Time Sinker | B9 | Async Batch Jobs with Callback | 2 | 4 | 4 | No | Origin is Engineering preference, the core use case is a synchronous 1,000-part call under 1,500 ms, and background processing sits outside that guardrail. |
| Time Sinker | B10 | Part Alternates Suggestions | 1 | 5 | 6+ | No | It has no tie to the moment of misery or any M2/M3 evidence, and it needs new substitute-part data and logic that exceed the whole sprint. |

## Quadrant Summary

| Quadrant | Features | Est. Eng-Weeks |
|---|---|---|
| Quick Win | B2, B5, B3, B1 | About 6 (uses all sprint capacity, no buffer) |
| Major Project | None | None |
| Fill-In | B7, B6 | About 3 |
| Time Sinker | B4, B8, B9, B10 | About 19 or more |
