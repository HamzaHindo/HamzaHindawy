---
id: 1
type: epic
title: "Portfolio foundation"
parent: initiative-portfolio
covers: ["R12", "R13"]
sources: []
companions: ["_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md"]
after: []
refined: true
risk: medium
---

# Portfolio foundation

## Description

The platform baseline the other epics build on: the site scaffold builds and runs with tests, basic traffic analytics record visits and conversions, and only Hamza can sign in to the admin area. Hosting is deliberately deferred to the launch epic (epic 7) by the owner's 2026-11-22 decision, so every check here runs on the local or dev build.

## Requirements

- R1 (parent R12): Only Hamza can access the admin area, through authentication; he is its only user.
- R2 (parent R13): Basic traffic analytics ships in version 1 so the success metrics — visitor-to-conversion, call-to-client, reply speed — are readable.

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
- companion — `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md`, spine for this epic: AD-4, AD-5 (stack: Next.js + Django + PostgreSQL), AD-6 (owns the whole auth surface: session cookie, `/api/auth/*`, CSRF/CORS config, one public origin), AD-8 (provisions the object-storage port), AD-9 (ports scaffold, captcha site-key config endpoint), AD-10 (owns the revalidation plumbing and path map), AD-11 (builds the shared SSR fetch layer, 3 s budget); conventions: error-code registry, analytics event names, `.env.example` ownership

## Notes

- Decision: 2026-11-22 — hosting deferred to the launch epic (owner call).
- Decision: 2026-11-22 — the build stack is no longer open here: the companion spine fixes it (AD-5 — Next.js 16 + Django 6.1 + DRF + PostgreSQL, Vercel as the owner-proposed working assumption ratified at epic 7); the scaffold locks the patch pins.
- Open question: Q4 — hosting platform; settles at epic 7, not here.
- Assumption: the build stack is picked in this epic's first story with the user — the PRD defers "hosting platform and stack" together (OQ-4), but a greenfield build needs a stack from the first line of code. *(Superseded in part, 2026-11-22: the spine picks the stack (AD-5); the scaffold story implements it.)*
- Decision: 2026-11-22 — tracer bullet is entry 1, the monorepo scaffold with one SSR page reading PostgreSQL through the API: the thinnest path through both deployables and the database. Entry 2 (analytics) follows as the least-certain story, because "recorded and readable" has to be demonstrable before hosting exists.
- Decision: 2026-11-22 — sequencing: the fetch layer and error registry (entry 3) precede auth (entry 4) and the object-storage port (entry 5), whose API errors go through its envelope; the object-storage port (entry 5) precedes revalidation plumbing and the captcha config endpoint (entry 6), which extend the integrations module it built; the tracer (entry 1) seeds the complete foundation-owned `.env.example` so no two stories create rows in it; the refactor sweep (entry 7) closes, waiting on every other entry. No closing end-to-end suite: the PRD carries no test plan (offered, declined by default pending an owner ask).
- Open question: how "a visit and a conversion event are recorded and readable" is demonstrated before hosting exists — Vercel Web Analytics only records on a hosted deployment and hosting is epic 7's; proposed: a dev recording path here, the dashboard read verified in production by epic 7.
