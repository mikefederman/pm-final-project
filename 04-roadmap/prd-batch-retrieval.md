Product Requirements Document

# Batch Retrieval for Part Identification Repository

**Author:** Mike Federman · **Status:** Draft · **Target:** High-Fidelity Batch Retrieval Prototype
**Scope:** Partial-Success Responses and Batch-Aware Quota (Must Have release)

## 1. The Big Picture

**Vision:** Let an Equipment Review Engineer complete a BOM-wide review of up to 1,000 parts through the sanctioned API in one request.

**Press Release:** Equipment Review Engineers, 41% of our users, have had no sanctioned path for BOM-wide reviews. Under delivery pressure, 49% loop single lookups and 33% are suspected of scraping the database or sharing unencrypted CSV dumps. Two failures push them off the API: one bad part number fails the whole request (31% of sessions end after an error or timeout, up from 18%), and per-part quota counting triggers rate limits (12% of sessions, up from 5%).

This release removes both. A batch returns every valid part, accounts for every submitted part, and flags each invalid one. A batch counts as one call against quota, so a 100+ part review is no longer blocked.

| Type | Metric | Baseline | Target |
|---|---|---|---|
| Primary | Share of sessions reaching BOM scale (100+ lookups) through the sanctioned API | 7% | 14% |
| Guardrail | Batch response time, up to 1,000 parts | Under 1,500 ms | Under 1,500 ms |

## 2. The Details

**User Stories**

| # | Story | Acceptance Criteria |
|---|---|---|
| 1 | As an Equipment Review Engineer, I want valid parts returned when some IDs are invalid, so that one bad part does not fail my review. | An invalid part never aborts the request. All valid parts return with a 200 (partial) status. |
| 2 | As an Equipment Review Engineer, I want every submitted part accounted for, so that no gap goes unnoticed in my review. | One entry per submitted part, marked Returned or Flagged. Flagged entries show a code. Entry count equals parts submitted. |
| 3 | As an Equipment Review Engineer, I want a batch to count as one call, so that rate limits do not block a 100+ part review. | Any batch up to 1,000 parts is charged 1 call. When quota is exhausted, the request returns 429 and no data. |

**Key Terms**

| Term | Definition |
|---|---|
| Partial-Success Responses (B2) | A batch request returns results for every valid part number and a flagged entry for each invalid one, so one bad part number does not fail the request. |
| Batch-Aware Quota (B3) | A batch request counts as one call against the engineer's quota, whether it holds 10 or 1,000 part numbers. |

**Screens to Build**

| Screen | UI Elements |
|---|---|
| 1. Batch Setup | Part list input, Load Demo BOM button (includes invalid part numbers), quota limit setting for testing, Submit button |
| 2. Batch Response | Status pill (200 partial or 429), response time, per-part table (part number, Returned or Flagged, code) |
| 3. Review Summary | Completion banner, flagged-parts list, Start Another Batch and Reset Quota buttons |

**Functional Requirements**

1. Process batches of up to 1,000 part numbers in one request.
2. An invalid part number does not abort the request; all valid parts return with a 200 (partial) status.
3. Return one entry per submitted part number, with status Returned or Flagged.
4. Flagged entries carry the code INVALID_PART and no part data.
5. Entry count equals submitted count in every response; no part is dropped silently.
6. Charge 1 call per batch at any size up to 1,000.
7. When quota is exhausted, return 429 and no part data.
8. Respond in under 1,500 ms for batches up to 1,000 parts.

**Smart Behaviors**

| Situation | Outcome |
|---|---|
| Some parts invalid, quota available | 200 (partial); valid parts returned, invalid parts flagged INVALID_PART |
| No parts invalid, quota available | 200; all parts returned |
| Every part invalid | 200; all parts flagged; no clean-success banner |
| Quota available, any batch size | Charge 1 call |
| Quota exhausted | 429; no data |

**Technical Constraints (What Not to Build):** No external APIs. No login. useState only. Fixed demo part numbers only.

## 3. The Logistics

**Features Out:** Deferred (Should): differentiated error codes, summary counts, remaining-quota display, retry guidance and zero charge on 429, 1,000-part cap message, audit log, BOM-scale session instrumentation, per-part messages. V2 (Could): additional codes, stale-data warning, retry-flagged helper, duplicate handling, quota usage view. Out: B5 BOM File Ingestion, B1 Slim Payload Mode, B4, B6, B7, B8, B9, B10, batches above 1,000, team quotas, automatic correction of invalid parts.

**Edge Cases**

- Empty input: Submit is disabled.
- Double-click on Submit: processed once.
- Every part invalid: flagged list shown, no clean-success banner.
- **Safety guard:** a flagged part shows only its code, never part data, and no part appears that was not submitted. The system never suggests or substitutes a part.

**Decision Log**

1. One generic code (INVALID_PART) ships now. Differentiated codes wait to keep the sprint inside 2 engineers and 1 designer for 3 weeks.
2. Only Must items ship. Audit log, summary counts, and instrumentation are deferred, so the first sprint delivers capability, not reporting.

**Evals**

| Target | Measure | Pass |
|---|---|---|
| Accuracy | 9 test cases (sizes 10, 250, 1,000 by 0%, 2%, 10% invalid) | Entry count equals submitted in 9 of 9; 100% of invalid parts flagged; quota charge is 1 in 9 of 9 |
| Time on Task | First-time tester submits a 250-part batch and names the flagged parts | Under 60 seconds |
| Safety Triggers | Count across all test cases | 0 fabricated fields on flagged parts; 0 silently dropped parts |
