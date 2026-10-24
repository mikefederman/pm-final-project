*Product Requirements Document*

# Batch Retrieval for Part Identification Repository

**Author:** Mike Federman · **Status:** Draft · **Target:** High-Fidelity Batch Retrieval Prototype
**Scope:** Partial-Success Responses and Batch-Aware Quota

## 1. The Big Picture

**Vision:** Let an Equipment Review Engineer complete a BOM-wide review of up to 1,000 parts through the sanctioned API in one request.

**Press Release:** Equipment Review Engineers, 41% of our users, have had no sanctioned path for BOM-wide reviews. Under delivery pressure, 49% loop single lookups and 33% are suspected of scraping the database or sharing unencrypted CSV dumps. Two failures push them off the API: one bad part number fails the whole request (31% of sessions end after an error or timeout, up from 18%), and per-part quota counting triggers rate limits (12% of sessions, up from 5%).

Partial-Success Responses return every valid part and flag each invalid one with an error code. Batch-Aware Quota counts a batch as one call. Together, engineers finish the review in one request and follow up only on flagged parts.

| Type | Metric | Baseline | Target |
|---|---|---|---|
| Primary | Share of sessions reaching BOM scale (100+ lookups) through the sanctioned API | 7% | 14% |
| Guardrail | Batch response time, up to 1,000 parts | Under 1,500 ms | Under 1,500 ms |

## 2. The Details

**User Stories**

| # | Story | Acceptance Criteria |
|---|---|---|
| 1 | As an Equipment Review Engineer, I want valid parts returned when some IDs are invalid, so that one bad part does not fail my review. | Returned plus flagged equals submitted. Each flag shows an error code. Response is 200 (partial). |
| 2 | As an Equipment Review Engineer, I want a batch to count as one call, so that rate limits do not block a 100+ part review. | Any batch of 10 to 1,000 parts adds 1 to the quota meter. A 429 appears only when the limit is reached. |
| 3 | As an Equipment Review Engineer, I want to see which parts were flagged and why, so that I resolve only those. | Every flagged part is listed with its code. None are hidden. |

**Key Terms**

| Term | Definition |
|---|---|
| Partial-Success Responses (B2) | A batch request returns results for every valid part number and an error code for each invalid one, so one bad part number does not fail the request. |
| Batch-Aware Quota (B3) | A batch request counts as one call against the engineer's quota, whether it holds 10 or 1,000 part numbers. |

**Screens to Build**

| Screen | UI Elements |
|---|---|
| 1. Batch Setup | Part list input (paste or sample BOM), invalid-part share control, quota limit control, B2 and B3 switches, quota meter, Submit button |
| 2. Batch Response | Status pill (200, 422, 429), tiles (Submitted, Returned, Flagged, Quota Charged), response time, results table with error codes |
| 3. Review Summary | Completion banner, reconciliation line, flagged-parts list, quota remaining, Start Another Batch and Reset Quota buttons |

**Functional Requirements**

1. Accept 10 to 1,000 part numbers per request; block submission above 1,000.
2. With B2 on, flag 100% of invalid parts with NOT_FOUND, OBSOLETE_PART, or MALFORMED_ID.
3. Returned plus flagged equals submitted in every response.
4. With B2 on, a batch containing invalid parts returns 200 (partial), never 422.
5. With B3 on, charge 1 call per batch at any size.
6. A 429 charges 0 calls and returns no data.
7. Show a response time under 1,500 ms for every batch.

**Smart Behaviors**

| Situation | Outcome |
|---|---|
| B2 on, some parts invalid | 200 (partial); valid parts returned, invalid parts flagged |
| B2 on, no parts invalid | 200; all parts returned |
| B3 on, quota available | Charge 1 call; continue to validation |
| Quota at limit | 429; no charge; no data |
| B2 off, some parts invalid | 422; whole batch rejected |
| Input above 1,000 or below 10 parts | Inline message; no request sent |

**Technical Constraints (What Not to Build):** No external APIs. No login. useState only. Fixed sample part numbers only.

## 3. The Logistics

**Features Out:** B8 Agent Connector SDK, B9 Async Batch Jobs, and B10 Part Alternates (cut). B4 Secure Bulk Export, B6 Audit Dashboard, and B7 Quickstart Kit (later). B5 BOM File Ingestion and B1 Slim Payload Mode (not in this prototype).

**Edge Cases**

- Empty input: Submit is disabled.
- Duplicate part numbers: counted once, with a note.
- Double-click on Submit: processed once.
- Every part invalid: no clean-success banner.
- **Safety guard:** an invalid part shows only its error code, never part data, and only the three defined codes can appear.

**Decision Log**

1. Ship B2 and B3 together as separate work items, since each alone leaves the engineer stuck in the other failure.
2. Accept pasted or sample part lists only. BOM upload (B5) is deferred to protect the 3-week sprint for 2 engineers and 1 designer.

**Evals**

| Target | Measure | Pass |
|---|---|---|
| Accuracy | 9 test cases (sizes 10, 250, 1,000 by 0%, 2%, 10% invalid) | 100% of invalid parts flagged; reconciliation holds in 9 of 9 |
| Time on Task | First-time tester submits a 250-part batch and reads the summary | Under 60 seconds |
| Safety Triggers | Count across all test cases | 0 fabricated fields on invalid parts; 0 charges on a 429 |
