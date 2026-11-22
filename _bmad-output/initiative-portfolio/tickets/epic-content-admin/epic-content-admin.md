---
id: 6
type: epic
title: "Content admin area"
parent: initiative-portfolio
covers: ["R9", "R10", "R11"]
sources: []
companions: []
after: ["epic-project-proof", "epic-contact-form", "epic-home-trust"]
refined: false
risk: medium
---

# Content admin area

## Description

The operator side: Hamza manages projects — scale figures, translation lines, technologies, live URL, screenshots, repo link — and the contact form's target email and the sections' text content through the authenticated admin area, without touching code. A project added here appears in the gallery (R9's growth part; its launch part is epic 3's). Deliberately last: the leanness fallback (Q7) can still defer R11's section-text editing to a 1.1 if the build overruns.

## Outcome

Every journey meets fresh evidence: Hamza keeps the proof current without a developer.

## Done when

- Hamza adds, edits and removes a project — including its figures, translation lines, technologies, live URL, screenshots and repo link — without touching code.
- The contact target email and the sections' text update through the admin area.
- A project added through the admin appears in the gallery.
- Only Hamza passes the sign-in; the authentication comes from epic 1.

## Boundaries

- Belongs to siblings: the pages it edits (epics 2–3), the contact form whose target it manages (epic 4), the authentication mechanism (epic 1).
- Out: a general-purpose CMS — no plugin ecosystem, no roles beyond Hamza.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Requirements R9, R10, R11
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, FR-11, FR-12, FR-14, FR-15

## Notes

- Waits on epic-project-proof because: it manages the project data model that epic builds.
- Waits on epic-contact-form because: it manages the target-email setting that epic owns.
- Waits on epic-home-trust because: it edits the sections' text on the page that epic builds.
- Open question: Q7 — whether the leanness fallback triggers (R11's section-text editing deferred to a 1.1) and who calls it; decided at MVP planning.
