---
id: 7
type: epic
title: "Launch: hosting and go-live"
parent: initiative-portfolio
covers: []
sources: []
companions: ["_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md"]
after: ["epic-project-proof", "epic-contact-form", "epic-discovery-booking", "epic-content-admin"]
refined: false
risk: medium
---

# Launch: hosting and go-live

## Description

The site goes live on the hosting settled after the build — the owner's 2026-11-22 call, which moved PRD OQ-4 behind the build: domain and production environment, first meaningful paint under 2 seconds on a mid-range phone, running cost inside the named ceiling, and the booking and form paths re-verified in production. Settles Q4 (hosting platform), Q9 (the cost ceiling) and Q5 (retention, region, EU copy) before go-live.

## Outcome

Real visitors reach a live site whose speed, cost and conversion paths are verified where they will actually run.

## Done when

- The site is reachable on its hosting and domain in production.
- First meaningful paint is under 2 seconds on a mid-range phone in production (C5).
- The booking and form paths are verified end to end in production, including delivery of messages and notifications (RULE-5, C6).
- The running cost is measured against the named ceiling (Q9 answered, C8).
- Retention and hosting region are stated, and EU obligations if any are answered (Q5).

## Boundaries

- Belongs to siblings: the build itself (epics 1–6); this epic deploys and verifies what they built, it does not rebuild it.
- Out: money on the site, the call itself — unchanged by go-live.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Constraints C5, C8; Open questions Q4, Q5, Q9
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, RULE-4, RULE-7, Integrations and operations, Data and compliance
- companion — `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md`, spine for this epic: AD-5 (ratifies the Vercel working assumption, weighing the Hobby plan's non-commercial ToS clause), AD-11 (owns keeping Django warm — uptime, keep-alive, monitoring), AD-12 (owns launch verification: dev + prod only, deploy = merge to main, one tested Postgres restore, uptime monitor on the API and conversion paths, the spine's 99% monthly availability target for booking + form); the spine's Deferred table names what settles here: Python host + region, Vercel ratification, against Q9's still-open cost ceiling

## Notes

- Decision: 2026-11-22 — hosting settled after the build (owner call); this epic owns it, overriding the PRD OQ-4 timing of "before the opening epic".
- Open question: Q4 — hosting platform; settles here.
- Open question: Q9 — the running-cost ceiling for RULE-7; owner: Hamza; needed at the hosting decision.
- Open question: Q5 — data retention and hosting region, and whether EU clients are in the audience; owner: Hamza; needed before launch.
