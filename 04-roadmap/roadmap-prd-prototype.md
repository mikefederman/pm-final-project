# Roadmap, PRD & Prototype (Module 4)

## Your strategic anchors
- **Persona (M2), who are you solving for?:** Equipment Review Engineers. They verify parts for equipment reviews under delivery pressure with no sanctioned bulk path.
- **Primary success metric (M3), your leading indicator:** Share of sessions reaching BOM scale (100+ lookups) through the sanctioned API. Baseline 7%; hypothesis target 14% (100% relative change).
- **Moment of misery (M2), the specific friction blocking the goal:** An engineer faces a BOM-wide review deadline with no sanctioned bulk retrieval path, so they loop single lookups or scrape the database and share unencrypted CSV dumps.
- **Guardrail metric (M3), what must not drop or break:** Daily active engineers (baseline 8,420) and batch response time under 1,500 ms.

## Scan the backlog & set a human baseline
- **My instinctive “quick wins” before touching the AI (2 to 3 feature IDs + why):** B5 BOM File Ingestion: Accepts a BOM file and converts it to a part-number batch request. Because this is a core function for equipment reviews.

B3 Batch-Aware Rate Limit Tier: Counts a batch request as one call against quota instead of up to 1,000. Because this is important for large multi-part lookups.

B1 Slim Payload Mode: Returns only requested part fields, with no frontend UI elements, to cut token volume. Because this will streamline the process.

## Audit, override & decide
- **Where did you override the AI? (feature + old vs. new score + why):** AI had four Quick Win features and three matched the three I identified has highest priority.
- **Did the AI over-value a Sales/Eng request your M2 interviews don’t support?:** No. The two requests with engineering or stakeholder origins were scored low and landed in Time Sinker.
- **Did it underweight something your M3 cohort/funnel data strongly supports?:** Possibly. Secure Bulk Export could be more valuable than a Time Sinker to avoid shadow workarounds.

## Generate your interactive roadmap
- **My “Now” lane (this sprint), the 2 to 3 quick wins I’ll build first:** BOM File Ingestion
Batch-Aware Rate Limit Tier
Slim Payload Mode
- **What I cut, and the “no” I’m protecting the scope from:** Part Alternates Suggestions
Async Batch Jobs with Callback	
Agent Connector SDK
- **Prototype/roadmap screenshot link (paste into your deliverables):** https://github.com/mikefederman/pm-final-project/blob/main/04-roadmap/roadmap-batch-retrieval.html (click the link, then in the GitHub HTML code screen, click the "Download raw file" arrow in the upper right-hand corner)
