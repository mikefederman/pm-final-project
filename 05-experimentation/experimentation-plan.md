# Experimentation Plan (Module 5)

## Get your documents ready
- **From M3, your hypothesis sentence:** If Equipment Review Engineers get batch retrieval through a sanctioned path, the share of pilot sessions submitting 100+ part numbers will rise from 7% to 14%, while daily active engineers (8,420) hold steady and batch responses stay under 1,500 ms.
- **From M3, your primary success metric & guardrail metric:** Primary: share of pilot sessions submitting 100+ part numbers. Guardrails: daily active engineers (8,420) and batch response time under 1,500 ms for up to 1,000 parts.
- **From M4, the feature you scoped in your PRD this is what you're testing:** Batch retrieval backend capability: Partial-Success Responses (B2), Batch-Aware Quota Counting (B3) and Automation Payload (B1).

## Define your experiment parameters
- **Feature under test pull from your M4 PRD:** One request of 1 to 1,000 part numbers. It returns every valid part in an automation payload and flags each invalid part as INVALID_PART without failing the batch. It accounts for every submitted part and is charged as 1 call.
- **Persona pull your M2 persona:** Equipment Review Engineer (41% of the engineer base). This engineer verifies parts for equipment reviews under delivery pressure. With no sanctioned bulk or automated path, they loop single lookups or use shadow workarounds (scraping data from the legacy UI, CSV dumps).
- **Expected outcome the behaviour change you expect, from your M3 hypothesis:** Engineers complete BOM-wide reviews through a single batch request instead of loops, scraping data from the legacy UI, or CSV dumps.
- **Primary success metric the one number that defines success, from M3:** 14% of pilot sessions submitting 100+ part numbers.
- **Baseline rate today's rate of your primary metric, from your M3 data:** 7% of legacy UI sessions reach 100+ lookups, down from 15% six months ago.
- **Guardrail metric & boundary what must not break, and how far it can move before you investigate:** Batch response time: over 1,500 ms is a breach. Daily active engineers (8,420): investigate a drop of more than 2%. Legacy p95 latency (620 ms): investigate a rise of more than 10%.
- **Minimum Detectable Effect (MDE) the smallest improvement worth shipping, your floor:** +3 percentage points (7% → 10%), the pilot's proceed threshold.
- **Sample size per arm use the calculator in the builder, baseline + MDE:** 1,356
- **Traffic split & test duration 50/50 standard · cover ≥ 2 weekly cycles:** 50/50 by engineer. The control group stays on the legacy path, and the test group uses the batch path.
- **Significance threshold p < 0.05 is standard, explain any deviation:** p < 0.05, two-sided. No deviation. The standard threshold fits a pilot whose result feeds a later production decision, so a false positive is costly but recoverable. Two-sided means a drop in the metric can also be detected, not only a rise.

## Define your control and variant
- **Control (A) the current experience, reference your M2 moment of misery and M3 funnel/workflow data:** See https://github.com/mikefederman/pm-final-project/blob/main/05-experimentation/ab-test-control-variant-isolation.md
- **Variant (B) your single change, copy the relevant screens & functional requirements from your M4 PRD:** See https://github.com/mikefederman/pm-final-project/blob/main/05-experimentation/ab-test-control-variant-isolation.md
- **Isolation check, what has NOT changed? list everything identical between arms (app version, recommendation engine, notifications, onboarding). If something changed inadvertently, your test is compromised.:** See https://github.com/mikefederman/pm-final-project/blob/main/05-experimentation/ab-test-control-variant-isolation.md

## Formalize your hypothesis & shipping criteria
- **Your hypothesis (filled in):** I believe that batch retrieval (partial-success responses, batch-aware quota counting and an automation payload) for Equipment Review Engineers will result in BOM-wide reviews completing through the sanctioned path instead of looped lookups or shadow workarounds, as measured by a 7-percentage-point increase (7% to 14%) in the share of pilot sessions submitting 100+ part numbers within 13 weeks. We will protect daily active engineers (8,420) and batch response time under 1,500 ms throughout the test.
- **Your shipping criteria (filled in):** We will SHIP if the share of sessions submitting 100+ part numbers improves by ≥ 3 percentage points (7% to 10%) at p < 0.05 (two-sided)
and batch response time does not exceed 1,500 ms, daily active engineers do not drop more than 2% (from 8,420) and legacy p95 latency does not rise more than 10% (from 620 ms) after 13 weeks.

We will ITERATE if direction is positive but lift is below the MDE.

We will KILL if the primary metric shows no improvement or moves negatively.

The read date is fixed at the end of 13 weeks, no results reviewed before this date.
- **Hardest parameter to define, and did it change your hypothesis? quick debrief:** **Hardest parameter:** the control group, together with the primary metric definition it depends on. The Module 3 and 4 pilot gave batch retrieval to the whole select team and compared it with the 7% legacy baseline, so there was no control. Setting one up exposed two more gaps. The legacy baseline counts *lookups*, while the batch path counts *part numbers submitted*. And a "session" was not defined for an automated workflow. The instrumentation needed to read the metric had also been deferred as out of scope for a basic POC.

**Did it change the hypothesis?** The claim did not change: the same persona, feature, metric, baseline and 14% target. What changed is how the hypothesis is tested. I added a counting rule that measures both paths in one unit, a 50/50 control group split by engineer, guardrail boundaries, and a fixed 13-week read date. Session counting also moved from a Should to a Must, so the earlier documents now lead into the test.
