---
id: 6
type: epic
title: "Content admin area"
parent: initiative-portfolio
covers: ["R9", "R10", "R11"]
sources: []
companions: ["_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md"]
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
- A read-only messages inbox lists stored messages — sender, time, body, delivery status — and nothing beyond listing (AD-3).
- A project added through the admin appears in the gallery.
- Only Hamza passes the sign-in; the authentication comes from epic 1.

## Boundaries

- Belongs to siblings: the pages it edits (epics 2–3), the contact form whose target it manages (epic 4), the authentication mechanism (epic 1).
- Out: a general-purpose CMS — no plugin ecosystem, no roles beyond Hamza.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Requirements R9, R10, R11
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, FR-11, FR-12, FR-14, FR-15
- companion — `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md`, spine for this epic: AD-2 (every DB write except the message intake goes through this epic's authenticated admin endpoints in the `admin` module; the SPA never writes the database and never holds storage credentials), AD-3 (a read-only messages inbox — sender, time, body, delivery status; nothing beyond listing), AD-8 (picks the storage vendor and fills credentials into epic 1's port; uploads multipart through Django; epic 3 seeds on the dev/local adapter until then, and epic 7 verifies the swap in production — per the spine's amended AD-8), AD-10 (calls the revalidation port after every successful save; extends the path map only by referencing existing keys); conventions: extends — never forks — epic 3's canonical Project schema, edits `CONTACT_SETTINGS` through the admin endpoint, edits `content` rows seeded by epic 2

## Notes

- Waits on epic-project-proof because: it manages the project data model that epic builds.
- Waits on epic-contact-form because: it manages the target-email setting that epic owns.
- Waits on epic-home-trust because: it edits the sections' text on the page that epic builds.
- Open question: Q7 — whether the leanness fallback triggers (R11's section-text editing deferred to a 1.1) and who calls it; decided at MVP planning.
- Decision: 2026-11-22 — the spine's AD-3 scope addition folded in: a read-only messages inbox joins this epic's admin area, beyond FR-11/FR-12; flagged by the architecture run, adopted with the spine as companion.
