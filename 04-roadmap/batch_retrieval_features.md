# Scenario: Batch Retrieval Feature Proposals (Simulated)

| # | Feature | What It Does | Origin |
|---|---|---|---|
| B1 | Slim Payload Mode | Returns only requested part fields, with no frontend UI elements, to cut token volume | M2 UXR (payload bloat) |
| B2 | Partial-Success Batch Responses | Returns results for valid parts plus per-part error codes, so one bad part number does not fail the request | M2 UXR (brittle error recovery); M3 Data (31% abandoned after error/timeout) |
| B3 | Batch-Aware Rate Limit Tier | Counts a batch request as one call against quota instead of up to 1,000 | M3 Data (429s 12% vs. 5%) |
| B4 | Secure Bulk Export | Encrypted, access-logged bulk download that replaces ad hoc CSV dumps | M3 Cohort (33% suspected workaround in Equipment Review Engineers) |
| B5 | BOM File Ingestion | Accepts a BOM file and converts it to a part-number batch request | M3 Cohort (Equipment Review Engineers, BOM-scale reviews) |
| B6 | Bulk Access Audit Dashboard | Shows sanctioned versus anomalous bulk-access activity | M3 Data (412 events per month, no baseline) |
| B7 | First-Batch Quickstart Kit | Sample batch call, test dataset, and guided first request | M3 Retention (largest drop at Mo. 0 to 1) |
| B8 | Agent Connector SDK | Pre-built client for AI agents and pipelines to call batch retrieval | M3 Cohort (Pipeline Automators, 66% looped) |
| B9 | Async Batch Jobs with Callback | Accepts large batches, processes in the background, and notifies on completion | Engineering |
| B10 | Part Alternates Suggestions | Recommends substitute parts when a retrieved part is obsolete or unavailable | Stakeholder Request |

## Notes

| Item | Note |
|---|---|
| Origin Labels | All origins are simulated labels mapped to the M2 research and M3 snapshots. Nothing here is interview evidence. |
| B7 Link | Linking the Mo. 0 to 1 drop to onboarding is carried over from the inference in Snapshot 2. |
| B9 Tension | Inference: background processing may sit outside the 1,500 ms response-time guardrail. |
| B9 and B10 | Inference: B9 and B10 were written as stakeholder-style requests, comparable to A8 and A10 in Scenario A. They are not tied to the M2 or M3 evidence. |
