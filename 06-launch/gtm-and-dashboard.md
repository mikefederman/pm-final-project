# GTM Strategy & Success Dashboard (Module 6)

## Get your prior work ready
- **The feature you're launching from your M4 PRD, one feature, not a list:** Batch Retrieval: a backend capability that lets an Equipment Review Engineer’s workflow retrieve up to 1,000 part numbers in one request. Every valid part comes back in an automation-ready payload, each invalid part is flagged without failing the batch, and the request counts as one call against quota.
- **What your M5 experiment told you shipped / iterating, the evidence behind the launch:** Feature status is not yet shipped. Launching as a controlled pilot, with the ship, iterate or kill decision set for the 13-week read date.
- **Your persona pull your M2 persona, this anchors your audience:** Equipment Review Engineers

## Set your goal, then your audience
- **Primary GTM goal awareness · engagement · conversion, pick one:** Engagement
- **Why this goal? what makes this the right goal for this feature right now:** The purpose of the pilot is to test the batch retrieval feature and reach decision whether to launch, iterate, or kill.
- **Target audience the specific segment your goal implies, be precise:** The target audience is a select group of engineers who will participate in the pilot.

## Size your launch
- **Launch tier S (minimal) · M (targeted) · L (multi-channel) · XL (full GTM):** M (targeted)
- **Justification reach + revenue impact + what silence would risk:** A select group of engineers will test and help evaluate the quality and functionality of the batch retrieval feature. They are both the pilot audience and the persona of who the feature is targeting.
- **Is this a launch or a release? does it need go-to-market, or can it just ship?:** Completion of the pilot will determine whether it can ship. The feature will be released (go to market) if the pilot results are positive.

## Choose your top three channels and plan assets
- **Channel 1 owned / earned / paid, and why it reaches your audience:** Owned: The persona builds workflows that call the API, so the portal is where they and their collaborators go when wiring a workflow. It’s also where quota and access already live (APIM). The legacy UI is ruled out as a channel, because the PRD keeps it unchanged.
- **Channel 2 owned / earned / paid, and why:** Earned: The audience trusts other engineers more than announcements. Pilot engineers can show a real BOM review done in one batch, with measured hours per review (S11). This directly answers the shadow-workaround habit, since peers show the sanctioned path working.
- **Channel 3 owned / earned / paid, and why:** Paid (internal equivalent: budgeted slots): It reaches the 41% of the base outside the pilot at a single moment, and it signals sanctioned status. That matters for engineers who use CSV dumps because nothing official exists. This is an inference, since an internal platform has no true paid media.
- **Enablement & assets what Sales / CS / Support need, plus the assets to build (one-pager, demo, etc.):** Enablement: support learns to read partial-success responses, INVALID_PART and 429 errors. Engineering managers get the case for switching. Pilot champions onboard new teams. InfoSec and APIM align on access and quota answers.

Assets: a shared messaging brief, a one-pager for managers, the existing prototype as a demo, a quick-start kit (B7, pulled forward from Later), a support runbook and FAQ, and a pilot results story after the 13-week read.

## Make it executable
- **Ownership named owner per key activity, individual, not department:** -
Launch decision, experiment read-out, messaging brief: PM
Batch endpoint build (B2, B3, B1): Engineer 1
Session counting (FR 10, M6) and guardrail monitoring: Engineer 2
Prototype demo, one-pager, quick-start kit (B7): Designer
Access control and vetting of about 100 pilot engineers: InfoSec lead
Quota values and developer portal listing: APIM lead
Support runbook and help desk training: Platform support lead
Peer demos and onboarding new teams: Pilot champion engineer
- **Budget & resource gaps what costs extra, and any asset you can't currently build:** -
Costs extra: the unestimated core endpoint, session counting (M6) and the N+1 fix could push the build past one sprint. Other extras are pulling the quickstart kit (B7) forward, InfoSec vetting for about 100 engineers, 13 weeks of team time to monitor the pilot, and small internal promotion slots.

Can’t build yet: the results story (needs the 13-week read), measured hours saved (time capture is deferred), final docs and sample call (the payload fields are undefined), a security visibility view (B6 and S6 are deferred) and named owners.

The first step is estimating the endpoint and M6, since nothing can be costed reliably until then.
- **Timeline Phase 1 beta → Phase 2 launch moment → Phase 3 post-launch:** -
Build (Weeks 1–3)
Build B2, B3, B1 and M6
Run PRD evals
InfoSec vets about 100 engineers
Gate: evals pass, response under 1,500 ms

Phase 1: Beta (Weeks 4–16)
50/50 split by engineer
Test group on the batch path
Control group on the legacy path
Communication to the test group only
Guardrails monitored throughout
No peeking at the primary metric
Gate: fixed read date at week 16

Phase 2: Launch Moment (About Week 17)
Read results
Apply ship, iterate or kill criteria
All-hands announcement
Developer portal listing
One-pager release
Pilot engineers’ results story
Gate: ship decision

Phase 3: Post-Launch (Week 18 Onward)
Release the quick-start kit (B7)
Peer demos
Onboard new teams
Monitor guardrails
Deliver Next items: security guardrails, Should list, quota policy
Deliver Later items: B5, B4, B6
Gate: general production release after security guardrails

## Define how you'll know it worked
- **Success metrics 2 to 3 metrics that match your GTM goal:** -
1. Share Of Sessions Submitting 100+ Part Numbers
Baseline: 7%
Target: 14% (10% to proceed)
Tracks BOM-wide reviews moving to the sanctioned path

2. Anomalous Bulk-Access Events Per Month
Baseline: 412
Target: decrease (no target set)
Tracks the decline of shadow workarounds
Read after launch, across the whole base

3. Daily Active Engineers (Guardrail)
Baseline: 8,420
Target: no decrease
Investigate a drop over 2%
Confirms the existing base isn’t pushed away
- **Bad signal to watch for e.g. high reach, zero signups = message-market mismatch:** Primary bad signal: high reach with a flat BOM-scale share. The all-hands, portal visits and quick-start downloads go up, but the share of sessions with 100+ part numbers stays near 7%.

What it means:
Engineers heard about the path but didn’t move their BOM reviews onto it.
The message lands, but the workflow doesn’t fit. The hypothesis doc calls this the pivot case: “the barrier is payload or workflow design rather than bulk retrieval.”

Additional signals to watch:
Batch usage rises, but anomalous bulk-access events stay near 412 a month: The batch path is being used in addition to workarounds, not instead of them. The security story isn’t landing.
Batch requests are mostly under 100 parts: Engineers treat batch as a faster single lookup, not a BOM-wide tool. The BOM-scale use case isn’t coming through.
- **Most likely post-launch decision double-down · iterate · pivot · deprioritize, and what would trigger it:** -
Most likely: Iterate. This is an inference based on the simulated Module 3 projections.

Why:
Snapshot 2 projects the batch path improving Month 1 retention by about 13 points.
The Month 0 → 1 drop still persists, at 23–28 points.
Module 3 attributes that leftover drop to onboarding and payload friction, not to bulk access.
The fixes for it (the B7 quick-start kit and a defined payload field set) are not in the pilot build.
