# MoSCoW: Combined Partial-Success Responses and Batch-Aware Quota

**Guiding Anchors:** Equipment Review Engineers; moment of misery (a BOM-wide deadline with no sanctioned bulk path, so they loop single lookups or scrape and share CSV dumps); Primary Metric (BOM-scale session share, 7% to 14%); Constraints (2 engineers + 1 designer, 3-week sprint).

**Test Applied:** A requirement is a Must only if removing it stops the persona from completing an automated BOM-wide workflow through the sanctioned API, or lets the workflow finish with undetected gaps.

## Must Have

| # | Sub-Requirement | Moment of Misery Link | Removal Test |
|---|---|---|---|
| M1 | **Failure isolation:** an invalid part number does not abort the batch. Valid parts return with a 200 (partial) status. | One bad ID currently sinks the whole BOM request | Workflow fails on the first bad part and falls back to loops or dumps |
| M2 | **Per-part result entries:** each part is keyed by part number, with a status (returned or flagged) and a reason code field. One generic invalid code is enough for the first sprint. | The engineer needs to know which parts to follow up | Workflow cannot act on flagged parts without identifying them |
| M3 | **No silent drops:** returned plus flagged equals submitted in every response. | A missing part in a BOM-wide review is the quality risk named in the M2 Risk Scale | Workflow completes with gaps nobody sees |
| M4 | **Batch-level quota charge:** quota is evaluated and charged as 1 call per batch, for any size up to 1,000. | Per-part counting triggers rate limits on 100+ part reviews (12% of sessions, up from 5%) | A BOM-scale batch is blocked before it returns data |

**Release Gates (apply to every Must, not separate requirements):** Response time under 1,500 ms for up to 1,000 parts including validation. Daily active engineers (baseline 8,420) not degraded, which can only be confirmed in a pilot.

## Should Have

| # | Sub-Requirement | Why It Is Not a Must |
|---|---|---|
| S1 | Differentiated codes: NOT_FOUND, OBSOLETE_PART, MALFORMED_ID | M2 works with one generic code, but distinct codes let the workflow route fixes |
| S2 | Summary counts in the response (submitted, returned, flagged, quota charged) | The workflow can count entries itself |
| S3 | On 429: charge 0, return no data, include Retry-After | A 429 already blocks the request. This keeps the engineer from being penalized for it |
| S4 | Remaining quota on every response | The workflow can still complete without it, but cannot plan around limits |
| S5 | Enforce the 1,000-part cap with an explicit rejection message | Promote to Must if the core endpoint does not already enforce it, since it protects the 1,500 ms guardrail |
| S6 | Audit log entry per batch (identity, part count, flagged count, time) | Addresses the security visibility gap and the 412 anomalous events per month, but the persona can finish without it |
| S7 | Instrumentation counting BOM-scale (100+) sessions through the sanctioned API | The persona does not need it, but the go/no-go decision cannot be made without it. Check this one first if capacity is tight. |
| S8 | Human-readable message per flagged part | Codes carry the same information for automated use |

## Could Have

| # | Sub-Requirement | Note |
|---|---|---|
| C1 | Additional codes (for example access restricted, temporarily unavailable) | Adds routing detail, not core function |
| C2 | Non-fatal stale-data warning per part | Inference: ties to the stale cached data bug in M2 research |
| C3 | Retry-flagged-only helper | Convenience for the follow-up step |
| C4 | Duplicate part numbers de-duplicated, with a count note | Edge-case polish |
| C5 | Engineer-facing quota usage view | Visibility, not function |

## Won't Have (Now)

| # | Exclusion | Reason |
|---|---|---|
| W1 | BOM file ingestion (B5) | Removes a manual input step but is not needed to complete the workflow. Its roadmap Should status is unchanged. |
| W2 | Slim Payload Mode (B1) | Targets token cost, not the moment of misery. Its roadmap Could status is unchanged. |
| W3 | Secure Bulk Export (B4), Audit Dashboard (B6), Quickstart Kit (B7) | Later (Fill-In) on the roadmap |
| W4 | Agent Connector SDK (B8), Async Batch Jobs (B9), Part Alternates (B10) | Cut (Time Sinkers) |
| W5 | Batches above 1,000 parts | Outside the stated key metric and guardrail |
| W6 | Team-level or pooled quotas | Users stay individuals for identity authentication, and snapshots exclude team-level adoption |
| W7 | Automatic correction or fuzzy matching of invalid part numbers | Safety exclusion: the system must flag bad parts, never guess substitutes |
| W8 | Changes to single-lookup behavior or its quota | Outside this feature |

## Notes

| Item | Note |
|---|---|
| Sprint Fit | Inference: the Musts sit within the earlier estimates for B2 and B3 (about 1.5 and 1 engineer-weeks, unvalidated), which leaves room for most Shoulds in the 6 engineer-week sprint. |
| Assumptions | The three code names and the core batch endpoint being in scope separately are carried from earlier. The M-level choice of a single generic code in M2 is my judgment. |
