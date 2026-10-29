# MoSCoW: Combined Partial-Success Responses, Batch-Aware Quota Counting and Automation Payload

*All research data in this document is simulated.*

**Guiding Anchors:** Equipment Review Engineers; moment of misery (a BOM-wide deadline with no sanctioned bulk or automated path, so they loop single lookups or use shadow workarounds: scraping data from the legacy UI and sharing CSV dumps); Primary Metric (share of pilot sessions submitting 100+ part numbers, 7% legacy baseline to 14% target); Pilot (a select team of engineers vetted for security; access control, activity logging and reporting assumed); Constraints (2 engineers + 1 designer, 3-week sprint, about 6 engineer-weeks).

**Test Applied:** A requirement is a Must only if removing it stops the workflow an engineer starts from completing a BOM-wide retrieval through the sanctioned path, lets the workflow finish with undetected gaps, leaves the response unusable for AI-assisted automation, or prevents the pilot from reading the primary metric (and so from testing the hypothesis).

## Must Have

| # | Sub-Requirement | Moment of Misery Link | Removal Test |
|---|---|---|---|
| M1 | **Failure isolation:** an invalid part number does not abort the batch. Valid parts return with a partial-success response. Batches of 1 to 1,000 parts are accepted, and a batch of one is a single lookup. | One bad ID currently sinks the whole BOM run | Workflow fails on the first bad part and falls back to loops or dumps |
| M2 | **Per-part result entries:** each part is keyed by part number, with a status (Returned or Flagged) and a reason code field. One generic invalid code (INVALID_PART) is enough for the first sprint. | The engineer needs to know which parts to follow up | Workflow cannot act on flagged parts without identifying them |
| M3 | **No silent drops:** Returned plus Flagged equals submitted in every response. | A part missing from a BOM-wide review goes unnoticed and becomes a quality risk | Workflow completes with gaps nobody sees |
| M4 | **Batch-level quota charge:** quota is evaluated and charged as 1 call per batch, for any size from 1 to 1,000. | Per-part counting triggers rate limits on 100+ part reviews (12% of requests, up from 5%) | A BOM-scale batch is blocked before it returns data |
| M5 | **Automation payload (B1 absorbed):** Returned entries carry the part data a workflow needs, with no frontend UI layout, image URLs or styling strings. A batch of one returns the same payload. | Each lookup returns a frontend-shaped payload of about 2,400 tokens that an AI-assisted workflow cannot use | Workflow builders must write code to strip UI layout, image URLs and styling from every response before automation can use it |
| M6 | **Pilot measurement (S7 promoted):** count sessions that retrieve 100+ part numbers on both the legacy path and the batch path, using one counting rule (a batch-path session is one workflow run). | The hypothesis claims the pilot moves engineers off loops and workarounds, which only this count can confirm | The pilot cannot read the primary metric, so the proceed, pivot or stop decision has no evidence |

**Release Gates (apply to every Must, not separate requirements):** Response time under 1,500 ms for up to 1,000 parts including validation. Daily active engineers (baseline 8,420) not degraded, which can only be confirmed in a pilot.

## Should Have

| # | Sub-Requirement | Why It Is Not a Must |
|---|---|---|
| S1 | Differentiated codes: NOT_FOUND, OBSOLETE_PART, MALFORMED_ID | M2 works with one generic code, but distinct codes let the workflow route fixes |
| S2 | Summary counts in the response (submitted, returned, flagged, quota charged) | The workflow can count entries itself |
| S3 | On 429: charge 0, return no data, include Retry-After | A 429 with no data already blocks the request. This keeps the engineer from being penalized for it |
| S4 | Remaining quota on every response | The workflow can still complete without it, but cannot plan around limits. Quota values stay with APIM. |
| S5 | Enforce the 1,000-part cap with an explicit rejection message | Promote to Must if the core endpoint does not already enforce it, since it protects the 1,500 ms guardrail |
| S6 | Audit log entry per batch (identity, part count, flagged count, time) | Addresses the security visibility gap, but the pilot assumes activity logging and reporting, so this is a general production release item |
| S7 | Moved to Must M6 | The pilot is a test of the hypothesis, so the measurement that reads the primary metric is required, not optional |
| S8 | Human-readable message per flagged part | Codes carry the same information for automated use |
| S9 | Caller-selectable fields (the workflow requests only the fields it needs) | M5 sets a fixed automation payload, which already removes the UI data. Field selection trims further. |
| S10 | Reject an empty batch with an error and no call charged (assumption) | Needed as a backend rule, but a workflow can avoid sending one |
| S11 | Capture engineering hours per BOM-wide review for pilot engineers, before launch and during the pilot | Measures the time benefit. The pilot gate does not depend on it. Baseline is 1 to 2 hours per review, or about 5,000 to 10,000 engineering hours annually in total (research-based assumptions). The number of reviewers is unknown and varies by week. |

## Could Have

| # | Sub-Requirement | Note |
|---|---|---|
| C1 | Additional codes (for example access restricted, temporarily unavailable) | Adds routing detail, not core function |
| C2 | Non-fatal stale-data warning per part | Inference: ties to the stale cached data bug in M2 research |
| C3 | Retry-flagged-only helper | Convenience for the follow-up step |
| C4 | Duplicate part numbers de-duplicated, with a count note | Edge-case polish |
| C5 | Engineer-facing quota usage view | Visibility, not function |
| C6 | Dynamic response compression (GZIP or Brotli) | Addresses a High M2 finding. Inference: it may help the 1,500 ms gate, but it is separate from M5. |

## Won't Have (Now)

| # | Exclusion | Reason |
|---|---|---|
| W1 | BOM file ingestion (B5) | Workflow-side step. The batch endpoint already accepts part numbers. Deferred (Later, Fill-In). |
| W2 | Secure Bulk Export (B4), Audit Dashboard (B6), Quickstart Kit (B7) | Later (Fill-In) on the roadmap |
| W3 | Agent Connector SDK (B8), Async Batch Jobs (B9), Part Alternates (B10) | Cut (Time Sinkers) |
| W4 | Batches above 1,000 parts | Outside the stated key metric and guardrail |
| W5 | Quota values, tiers and per-user application; team-level or pooled quotas | Set through APIM policy. M4 covers only the counting rule. |
| W6 | Automatic correction or fuzzy matching of invalid part numbers | Safety exclusion: the system must flag bad parts, never guess substitutes |
| W7 | Changes to the legacy UI or legacy single-lookup behavior, and legacy UI divestment | Outside this feature. Divestment is a separate path. |
| W8 | Security controls, access control design and activity logging build | Assumed for the pilot, with InfoSec owning the work |

## Notes

| Item | Note |
|---|---|
| Sprint Fit | Inference: M1 to M4 map to B2 (about 1.5 engineer-weeks) and B3 (about 1), and M5 maps to B1 (about 1), for about 3.5 of 6 engineer-weeks (unvalidated). M6 (pilot measurement) and the core batch endpoint have no estimate, so the remaining capacity is not a confirmed buffer. |
| M6 Placement | My judgment. M6 passes the Must test through the pilot's purpose, not the persona's workflow: the pilot exists to test the hypothesis, and the primary metric cannot be read without it. The Module 5 A/B test depends on it. |
| M5 Placement | My judgment. M5 passes the Must test through automation and engineering time (builders would otherwise strip UI data themselves), not through workflow completion, since a workflow could still finish with a UI-shaped payload. Token reduction is a benefit, not a driver. |
| M5 Dependency | The automation payload field set must be defined before build. The five prototype fields (Part Number, Description, Manufacturer, Category, Revision) are an illustration, not the definition. |
| Assumptions | The three code names and the core batch endpoint being in scope separately are carried from earlier. The choice of a single generic code in M2 is my judgment. |

## Changes From The Previous MoSCoW

| Item | Previous | Revised |
|---|---|---|
| B1 Slim Payload Mode | Won't (W2) | Must (M5), absorbed into the core |
| Status wording | "200 (partial)" | Partial-success response |
| Batch size | Up to 1,000 | 1 to 1,000, with a batch of one as a single lookup |
| S7 Session instrumentation | Should (deferred from the POC build) | Must (M6), because the pilot must read the primary metric |
| Should list | S1 to S8 | Added S9 (caller-selectable fields), S10 (empty batch) and S11 (time-per-review measurement). S6 and S7 now reflect the pilot's assumed logging and reporting. |
| Could list | C1 to C5 | Added C6 (dynamic compression) |
| Won't list | B5 (Should on roadmap), B1 | B5 deferred as workflow-side. Added quota policy (APIM), legacy UI and security (InfoSec). |
