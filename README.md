# My Product: Product Management Final Project

> My final project for Product School's **Product Management** certification. One product concept, **Batch Retrieval for Part Identification Repository** (my own initiative), taken from a raw problem to a launch-ready concept across six in-class labs, no homework required.

The deliverables are available in their module folders. The final project was created outside the Final Presentation file, so that file is just a template.

---

## Deliverables at a glance

| # | Deliverable | Module | Status | File |
|---|---|---|---|---|
| 1 | **Strategic Discovery Map** (groundwork) | M1 | ☐ | `01-product-thinking/strategic-map.md` |
| 1 | **Problem Hook & Value Proposition** (+ hypothesis) | M1 | ☐ | `01-product-thinking/problem-hook.md` |
| 2 | **AI Synthesis** — Product Health & Insights Summary | M2 | ☐ | `02-discovery/ai-synthesis.md` |
| 2 | **Competitive Analysis & Journey Map** | M2 | ☐ | `02-discovery/competitive-and-journey.md` |
| 3 | **Hypothesis & Success Metrics** | M3 | ☐ | `03-analytics/hypothesis-and-metrics.md` |
| 4 | **Roadmap, PRD & Prototype** | M4 | ☐ | `04-roadmap/roadmap-prd-prototype.md` |
| 4 | **PRD & Prototype Sprint** | M4 | ☐ | `04-roadmap/prd-and-prototype.md` |
| 5 | **Experimentation Plan** | M5 | ☐ | `05-experimentation/experimentation-plan.md` |
| 6 | **GTM Strategy & Success Dashboard** | M6 | ☐ | `06-launch/gtm-and-dashboard.md` |
| 6 | **Individual Insights** (reflection) | M6 | ☐ | `06-launch/individual-insights.md` |
| ★ | **Final Project Presentation** (the deck you submit) | M6 | ☐ | `06-launch/final-presentation.md` |

## The project

**Product concept:** Batch Retrieval for the Part Identification Repository. It is a backend capability that lets an engineer’s automated workflow retrieve 1 to 1,000 part numbers in one request. Every valid part comes back in an automation-ready payload, each invalid part is flagged without failing the batch, and the request counts as one call against quota.

**Who it’s for:** Equipment Review Engineers, 41% of the engineer base. They verify parts for equipment reviews under delivery pressure. With no sanctioned bulk or automated path, they loop single lookups or fall back on shadow workarounds such as scraping data from the legacy UI and sharing CSV dumps.

**The single bet:** that engineers turn to workarounds because the sanctioned bulk path is missing, not by preference. If that path exists, BOM-wide reviews will move onto it. This is measured by the share of pilot sessions submitting 100+ part numbers rising from 7% to 14%, while daily active engineers and response time hold.

___

## How to submit

- Turn the deliverable files into your final deck: run the **Final Presentation Generator** (linked from the Module 6 deck) to build a single-file HTML deck, or paste your files into an AI tool like **Gamma** or **Canva** (see `06-launch/final-presentation.md`).
- Commit the deck to this repo, then submit your **repo link** and your **presentation deck** to the learning platform within **7 days** of your cohort ending.

## Repo structure

```
pm-final-project/
├── README.md                              ← this dashboard
├── 01-product-thinking/
│   ├── strategic-map.md                   ← M1 lab 1: strategic discovery map
│   └── problem-hook.md                    ← M1 lab 2: problem hook + value prop ★ Deliverable 1
├── 02-discovery/
│   ├── ai-synthesis.md                    ← M2 lab 1: product health & insights summary
│   └── competitive-and-journey.md         ← M2 lab 2: workaround + journey map ★ Deliverable 2
├── 03-analytics/
│   └── hypothesis-and-metrics.md          ← M3: hypothesis + success metrics ★ Deliverable 3
├── 04-roadmap/
│   ├── roadmap-prd-prototype.md           ← M4 lab 1: roadmap + prioritization ★ Deliverable 4
│   └── prd-and-prototype.md               ← M4 lab 2: PRD + prototype
├── 05-experimentation/
│   └── experimentation-plan.md            ← M5: experimentation plan          ★ Deliverable 5
└── 06-launch/
    ├── gtm-and-dashboard.md               ← M6: GTM plan + success dashboard  ★ Deliverable 6
    ├── individual-insights.md             ← M6: friction, learnings, aha
    └── final-presentation.md              ← M6: how to build & submit the deck ★ Final submission
```
