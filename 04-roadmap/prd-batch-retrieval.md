Product Requirements Document

# Batch Retrieval for Part Identification Repository

Author: Mike Federman | Status: Draft | Target: Pilot Release With A Select Team Of Engineers  
Scope: Batch Retrieval Backend Capability (Partial-Success Responses, Batch-Aware Quota Counting, Automation Payload)  
*All research data in this document is simulated.*

## 1. The Big Picture

**Vision:** Let an Equipment Review Engineer start a workflow that retrieves up to 1,000 parts in one request through a sanctioned backend path, so a BOM-wide review finishes on schedule with less manual engineering time.

**Press Release:** Equipment Review Engineers, 41% of the engineer base, can only retrieve parts one at a time in the legacy UI. Under delivery pressure, 49% of their access is looped single lookups and 33% is suspected shadow workarounds: scraping data from the legacy UI or sharing unencrypted CSV dumps. Two failures push them off the sanctioned path. One bad part number fails the whole run (31% of sessions end after an error or timeout, up from 18%), and per-part quota counting triggers rate limits (12% of requests, up from 5%). Manual BOM-wide reviews take 1 to 2 hours each, approximately 5,000 to 10,000 engineering hours per year, and each lookup returns a frontend-shaped payload of about 2,400 tokens. This release adds a backend path for automated workflows. A batch of up to 1,000 parts returns every valid part in a payload built for automation, accounts for every submitted part, flags each invalid one, and counts as one call against quota. A select team of engineers pilots it.

### Success Metrics

| Type | Metric | Baseline | Target |
|---|---|---|---|
| Primary | Share of pilot sessions submitting 100+ part numbers | 7% (legacy UI sessions with 100+ lookups) | 14% (10% to proceed) |
| Benefit | Engineering hours per BOM-wide review | 1 to 2 hours per review; about 5,000 to 10,000 per year in total (research-based assumption) | Measured once time capture is in place; no threshold |
| Benefit | Tokens per lookup response | 2,400 | At least 75% lower (benefit) |
| Companion | Batches that account for every submitted part | N/A (new capability) | 100% |
| Guardrail | Batch response time, up to 1,000 parts | N/A (new capability) | Under 1,500 ms (over is a breach) |
| Guardrail | Daily active engineers | 8,420 | No decrease (investigate a drop over 2%) |
| Guardrail | Legacy single-lookup latency (p95) | 620 ms | Investigate a rise over 10% |

Counting rule (proposal): a session counts toward the primary metric when it retrieves 100+ part numbers, as legacy lookups or as part numbers submitted in batches. On the batch path, a session is one workflow run (assumption). Guardrail investigation thresholds are proposals.

## 2. The Details

### User Stories

| # | Story | Acceptance Criteria |
|---|---|---|
| 1 | As an Equipment Review Engineer, I want the workflow I start to return valid parts when some IDs are invalid, so that one bad part does not fail my review. | An invalid part never aborts the request. All valid parts return with a partial-success response. |
| 2 | As an Equipment Review Engineer, I want every submitted part accounted for, so that no gap goes unnoticed in my review. | One entry per submitted part, marked Returned or Flagged. Flagged entries show a code. Entry count equals parts submitted. |
| 3 | As an Equipment Review Engineer, I want a batch to count as one call, so that rate limits do not block a 100+ part review. | Any batch up to 1,000 parts is charged 1 call. When quota is exhausted, the request returns 429 and no data. |
| 4 | As an Equipment Review Engineer, I want returned parts in a payload built for automation, so that my AI-assisted workflow can process them without extra clean-up. | Returned entries carry no UI layout, image URL or styling data. A batch of one returns the same payload. Token volume is a measured benefit. |

### Key Terms

| Term | Definition |
|---|---|
| Workflow | An automated process, built by the engineer and collaborators, that the engineer starts and that calls the batch path. |
| Batch | One request carrying 1 to 1,000 part numbers. A single lookup is a batch of one. |
| Partial-Success Responses (B2) | A batch returns results for every valid part number and a flagged entry for each invalid one, so one bad part number does not fail the request. |
| Batch-Aware Quota Counting (B3) | A batch counts as one call against the applicable quota, whether it holds 1 or 1,000 part numbers. Quota values are set through APIM. |
| Automation Payload (B1) | The part data a workflow needs to process a lookup, with no frontend UI elements. |
| Returned / Flagged | Returned: a valid part with its data. Flagged: an invalid part with a code and no part data. |

### Illustrative Contract (Assumption)

Requirements stay at behavior level. This sketch shows what a workflow would code against. Field names are author-defined.

| Element | Illustration |
|---|---|
| Request | A list of 1 to 1,000 part numbers |
| Batch-level response | A status value, such as partial when any entry is Flagged. The final signal is an engineering decision. |
| Entry, Returned | Part Number, Description, Manufacturer, Category, Revision (illustrative subset of the automation payload) |
| Entry, Flagged | Part number, status Flagged, code INVALID_PART, no part data |

### Screens to Build (Demonstration Client Only)

The product is the backend capability. These screens demonstrate its behavior and are not product scope. The legacy UI is unchanged. The demonstration client does not demonstrate FR 10: session counting is backend instrumentation that runs in the background on both paths for the length of the pilot.

| Screen | UI Elements |
|---|---|
| 1. Batch Request | Part list input (custom part numbers accepted), Load Demo BOM button (includes invalid part numbers), batch limit test setting, Run Batch button |
| 2. Batch Response | Status pill (partial success or 429), response time (simulated), per-part table (part number, Returned or Flagged, code or five payload fields) |
| 3. Batch Summary | Result banner, flagged-parts list, Start Another Batch and Reset Quota buttons |

### Functional Requirements

1. Process batches of 1 to 1,000 part numbers in one request.
2. An invalid part number does not abort the request; all valid parts return with a partial-success response.
3. Return one entry per submitted part number, with status Returned or Flagged.
4. Flagged entries carry the code INVALID_PART and no part data.
5. Entry count equals submitted count in every response; no part is dropped silently.
6. Charge 1 call per batch at any size up to 1,000.
7. When quota is exhausted, return 429 and no part data.
8. Respond in under 1,500 ms for batches up to 1,000 parts.
9. Returned entries carry the automation payload, with no UI layout, image URL or styling data.
10. Count sessions that retrieve 100+ part numbers on both the legacy path and the batch path, using the counting rule, so the pilot can read the primary metric for each group.

### Smart Behaviors

| Situation | Outcome |
|---|---|
| Some parts invalid, quota available | Partial-success response; valid parts returned, invalid parts flagged INVALID_PART |
| No parts invalid, quota available | Success; all parts returned |
| Every part invalid | All parts flagged; no clean-success signal |
| Batch of one part | One entry, Returned or Flagged; charge 1 call |
| Quota available, any batch size | Charge 1 call |
| Quota exhausted | 429; no data |

**Technical Constraints (What Not to Build):** Backend: no change to the legacy UI or legacy single lookups; no security controls, logging or quota values (assumed or owned elsewhere). Demonstration client: no external APIs, no login, useState only. Demo data only: valid parts are PN-00001 to PN-02000 (assumption).

## 3. The Logistics

### Assumptions And Dependencies

- The feature is piloted with a select team of engineers vetted for security.
- Access control, activity logging and reporting are assumed, not built (InfoSec).
- Quota values and per-user application are set through APIM. A blanket quota is likely (assumption).
- The 1,500 ms gate depends on backend query efficiency. N+1 query degradation is a known risk.
- The automation payload field set is to be defined.
- Counting BOM-scale sessions on both paths is in pilot scope (FR 10, MoSCoW M6), because the pilot tests the hypothesis. Its effort is not yet estimated. Capturing review time remains deferred (Should).
- The pilot team is split 50/50 by engineer: a control group stays on the legacy path and a test group uses the batch path (proposal).

**Features Out:** Deferred (Should): differentiated error codes, summary counts, remaining-quota display, retry guidance and zero charge on 429, 1,000-part cap message, audit log, per-part messages, caller-selectable fields, empty-batch rejection, time-per-review capture. V2 (Could): additional codes, stale-data warning, retry-flagged helper, duplicate handling, quota usage view, dynamic response compression. Out: B5 BOM File Ingestion (workflow-side), B4, B6, B7, B8, B9, B10, batches above 1,000, team quotas, automatic correction of invalid parts, legacy UI changes and divestment, security controls.

### Edge Cases

- Empty input in the demonstration client: Run Batch is disabled.
- Double-click on Run Batch in the demonstration client: processed once.
- Every part invalid: flagged list shown, no clean-success signal.
- Safety guard: a flagged part shows only its code, never part data, and no part appears that was not submitted. The system never suggests or substitutes a part.
- Legacy UI and legacy single lookups behave as before.

### Decision Log

1. One generic code (INVALID_PART) ships now. Differentiated codes wait to keep the sprint inside 2 engineers and 1 designer for 3 weeks.
2. Only core items ship. Audit log and summary counts are deferred, so the first sprint delivers capability, not reporting. Session counting ships (FR 10), because without it the pilot cannot test the hypothesis.
3. The feature is a backend capability for workflows that engineers start. The legacy UI is unchanged.
4. A single lookup is a batch of one, so no separate single-lookup feature is built.
5. The automation payload is core scope because workflows need it. It replaces the optional Slim Payload Mode. Token reduction is a benefit, not a driver.
6. Quota values and per-user application are deferred to APIM policy. A blanket quota is the likely approach (assumption).
7. Requirements stay at behavior level, with an illustrative contract. The partial-success signal is an open engineering decision.
8. The pilot decision follows about 3 pilot months. Proceed if the pilot share reaches 10% (14% target), batches account for every submitted part, and guardrails hold. Pivot if batches complete but the share stays below 10%. Stop if batches fail or guardrails are breached. The decision rests on an A/B comparison (proposal): 50/50 by engineer, about 13 weeks, minimum detectable effect +3 points (7% to 10%), p < 0.05 two-sided, 80% power. A reading counts once each group has about 1,356 sessions, which is about 100 engineers at 2.3 sessions per engineer per week (inference). InfoSec vetting of that team size is to be confirmed.

### Evals

| Target | Measure | Pass |
|---|---|---|
| Accuracy | 11 test cases: 9 batches (sizes 10, 250, 1,000 by 0%, 2%, 10% invalid) plus 2 batches of one (valid, invalid) | Entry count equals submitted in 11 of 11; 100% of invalid parts flagged; quota charge is 1 in 11 of 11 |
| Payload | Returned entries across all test cases | 0 UI layout, image URL or styling fields; token volume per entry measured against the 2,400 baseline (at least 75% lower is a benefit, not a pass condition) |
| Performance | A 1,000-part batch | Response under 1,500 ms (simulated in the demonstration client) |
| Measurement | Seeded sessions on each path (99, 100 and 1,000 part numbers) | Sessions of 100 and 1,000 counted, 99 not counted, on both paths |
| Safety Triggers | Count across all test cases | 0 fabricated fields on flagged parts; 0 silently dropped parts |

