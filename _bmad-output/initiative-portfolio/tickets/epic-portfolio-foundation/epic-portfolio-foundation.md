---
id: 1
type: epic
title: "Portfolio foundation"
parent: initiative-portfolio
covers: ["R12", "R13"]
sources: []
companions: []
after: []
refined: false
risk: medium
---

# Portfolio foundation

## Description

The platform baseline the other epics build on: the site scaffold builds and runs with tests, basic traffic analytics record visits and conversions, and only Hamza can sign in to the admin area. Hosting is deliberately deferred to the launch epic (epic 7) by the owner's 2026-11-22 decision, so every check here runs on the local or dev build.

## Outcome

Hamza's site has a runnable, testable base that measures its own conversions and locks the admin area to him alone.

## Done when

- The scaffold builds and runs with its checks green.
- A visit and a conversion event are recorded and readable — the conversion events themselves arrive in epics 4 and 5.
- A sign-in exists that only Hamza can pass; anyone else is refused.

## Boundaries

- Belongs to siblings: the site's pages (epics 2–3), the conversion flows (epics 4–5), the content management screens (epic 6), hosting and deployment (epic 7).
- Touch point: the analytics service — the basic traffic analytics are chosen and wired here; owner: this epic.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Requirements R12, R13
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, FR-15, MVP scope

## Notes

- Decision: 2026-11-22 — hosting deferred to the launch epic (owner call).
- Open question: Q4 — hosting platform; settles at epic 7, not here.
- Assumption: the build stack is picked in this epic's first story with the user — the PRD defers "hosting platform and stack" together (OQ-4), but a greenfield build needs a stack from the first line of code.
