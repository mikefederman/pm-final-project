# Roadmap, PRD & Prototype (Module 4)

## Your strategic anchors
- **Persona (M2), who are you solving for?:** Equipment Review Engineers (41% of the engineer base). They verify parts for equipment reviews under delivery pressure with no sanctioned bulk or automated path.
- **Primary success metric (M3), your leading indicator:** Share of pilot sessions submitting 100+ part numbers. Baseline 7% (legacy UI sessions with 100+ lookups); hypothesis target 14% (100% relative change), with 10% as the proceed threshold.
- **Moment of misery (M2), the specific friction blocking the goal:** An engineer faces a BOM-wide review deadline with no sanctioned bulk or automated path, so they loop single lookups or fall back on shadow workarounds: scraping data from the legacy UI and sharing unencrypted CSV dumps.
- **Guardrail metric (M3), what must not drop or break:** Daily active engineers (baseline 8,420; investigate a drop over 2%) and batch response time under 1,500 ms for up to 1,000 parts.

## Scan the backlog & set a human baseline
- **My instinctive “quick wins” before touching the AI (2 to 3 feature IDs + why):** The following:

B5 BOM File Ingestion: Accepts a BOM file and converts it to a part-number batch request. Because this is a core function for equipment reviews.

B3 Batch-Aware Rate Limit Tier: Counts a batch request as one call against quota instead of up to 1,000. Because this is important for large multi-part lookups.

B1 Slim Payload Mode: Returns only requested part fields, with no frontend UI elements, to cut token volume. Because this will streamline the process.

## Audit, override & decide
- **Where did you override the AI? (feature + old vs. new score + why):** AI had three Quick Win features. Two matched the three I identified has highest priority. My feature that did not match was ultimately determined to be not a requirement to complete a batch lookup. I let AI override my decision because AI's top feature is mandatory for success. I treated this like a discussion with the architect. The final choice is the technically accurate decision.
- **Did the AI over-value a Sales/Eng request your M2 interviews don’t support?:** No. The two requests with engineering or stakeholder origins were scored low and landed in Time Sinker.
- **Did it underweight something your M3 cohort/funnel data strongly supports?:** Possibly. Secure Bulk Export could be more valuable than a Time Sinker to avoid shadow workarounds. I requested this feature be promoted to Later.

## Generate your interactive roadmap
- **My “Now” lane (this sprint), the 2 to 3 quick wins I’ll build first:** Partial-Success Batch Responses, Batch-Aware Quota Counting, Slim Payload Mode
- **What I cut, and the “no” I’m protecting the scope from:** Agent Connector SDK, Async Batch Jobs with Callback, Part Alternates Suggestions
- **Prototype/roadmap screenshot link (paste into your deliverables):** Access the feature roadmap, PRD, and prototype from the Support Files at https://github.com/mikefederman/pm-final-project/blob/main/06-launch/support-files.md.
