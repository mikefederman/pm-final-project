# A/B Test Design: Control, Variant And Isolation Check

*All research data is simulated. Items marked Inference are not supported by the research.*

## Control (A): Current Experience

| Element | Definition |
|---|---|
| Path | Legacy UI with 1:1 single-part lookups. There is no sanctioned bulk or automated path. |
| Moment Of Misery (M2) | Facing a BOM-wide review deadline, the engineer loops through parts one at a time or falls back on shadow workarounds (scraping data from the legacy UI, unencrypted CSV dumps). |
| Funnel (M3, Snapshot 1) | Session start 100% → lookup initiated 83% → record retrieved 69% → 10+ lookups 38% → **BOM-scale (100+) 7%** |
| Friction Data (M3) | 31% of sessions abandoned after error or timeout. 12% of requests rate-limited (429). Legacy p95 latency 620 ms. Payload about 2,400 tokens per lookup. |
| Persona Access Mix (M3, Snapshot 3) | 18% single lookups, 49% looped lookups, 33% suspected workaround |

## Variant (B): Single Change

**Change:** access to the batch retrieval path (B2 Partial-Success Batch Responses, B3 Batch-Aware Quota Counting, B1 Automation Payload) in addition to the legacy UI.

### Functional Requirements (M4 PRD)

| # | Requirement |
|---|---|
| FR 1 | Process batches of 1 to 1,000 part numbers in one request. |
| FR 2 | An invalid part number does not abort the request; all valid parts return with a partial-success response. |
| FR 3 | Return one entry per submitted part number, with status Returned or Flagged. |
| FR 4 | Flagged entries carry the code INVALID_PART and no part data. |
| FR 5 | Entry count equals submitted count in every response; no part is dropped silently. |
| FR 6 | Charge 1 call per batch at any size up to 1,000. |
| FR 7 | When quota is exhausted, return 429 and no part data. |
| FR 8 | Respond in under 1,500 ms for batches up to 1,000 parts. |
| FR 9 | Returned entries carry the automation payload, with no UI layout, image URL or styling data. |

### Screens (M4 PRD, Demonstration Client Only)

| Screen | UI Elements |
|---|---|
| 1. Batch Request | Part list input, Load Demo BOM button, batch limit test setting, Run Batch button |
| 2. Batch Response | Status pill (partial success or 429), response time, per-part table |
| 3. Batch Summary | Result banner, flagged-parts list, Start Another Batch and Reset Quota buttons |

The product is a backend capability. The screens demonstrate its behavior, and engineers in Variant B reach the path through their own workflows. FR 10 (session counting) applies to both arms, so it is listed under the isolation check, not here.

## Isolation Check: Identical In Both Arms

| Element | Both Arms |
|---|---|
| Legacy UI and single lookups | Unchanged and available (MoSCoW W7) |
| Part data source | Same repository and same part records |
| Invalid-part handling | No correction or substitution in either arm (MoSCoW W6) |
| Access control and vetting | Same InfoSec controls. Both arms drawn from the same vetted team. |
| Activity logging and reporting | Same, as assumed for the pilot |
| Quota limit values | Same APIM values. Only the counting rule differs (B3 is part of the change). |
| Measurement | Same counting rule and instrumentation (PRD FR 10, MoSCoW M6) |
| Onboarding and documentation | None added for either arm. The quickstart kit (B7) is deferred. |
| Notifications and communications | No pilot announcements beyond those needed to grant access |
| Compression and performance work | None added (C6 deferred) |
| Test window | The same 13 weeks for both arms |

## Watch For: Ways The Test Could Be Compromised (Inference)

| Risk | Why |
|---|---|
| Shared workflows | Workflows may be built with collaborators. A Variant B workflow shared with a Control engineer moves that engineer into B. |
| Extra support for Variant B | Hands-on help or documentation given only to the test group adds a second change. |
| Load spillover | Batch traffic on the shared backend could slow the legacy path for Control. The legacy p95 guardrail watches for this. |
| Unequal groups | Randomization by engineer should balance workload, review deadlines and seniority across the arms. |
