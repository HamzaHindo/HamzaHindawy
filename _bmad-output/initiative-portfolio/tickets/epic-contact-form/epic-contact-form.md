---
id: 4
type: epic
title: "Contact form and email delivery"
parent: initiative-portfolio
covers: ["R5"]
sources: []
companions: ["_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md"]
after: ["epic-home-trust"]
refined: false
risk: medium
---

# Contact form and email delivery

## Description

The fallback conversion: a visitor not ready to book submits the form at the end of the scroll and sees instant on-screen confirmation, and the message reaches Hamza's email with delivery failure surfaced through a second channel. The captcha guard is introduced here — a scripted burst of fake submissions is blocked while a genuine message completes with no more than the guard's single check. Settles the email service (Q3), which also carries the booking notifications epic 5 sends.

## Outcome

A visitor not ready to book can still start the conversation, and Hamza can trust that every submission arrived.

## Done when

- A submitted message arrives in Hamza's email (RULE-5's delivery check).
- The visitor sees instant on-screen confirmation on submit.
- A delivery failure surfaces to Hamza through a second channel; nothing is silently lost.
- A scripted burst of fake submissions is blocked while a genuine message completes with no more than the guard's single check.
- The email service decision is recorded (Q3).

## Boundaries

- Belongs to siblings: booking notifications through the same service (epic 5), editing the target email without code (epic 6), the captcha guard's reuse on the booking path (epic 5).
- Touch point: the email delivery service — chosen and owned here; owner: this epic.
- Touch point: Hamza's email inbox — the form's messages arrive there.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Requirement R5
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, FR-7, FR-8, RULE-5, RULE-6, UJ-3
- companion — `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md`, spine for this epic: AD-2 (the public surface's only write endpoint is the captcha-guarded `POST /api/messages/` through the `messaging` module), AD-7 (the delivery-status rule: `pending | sent | failed` on `messaging.Message`, the inbox renders it, a failed send also triggers the best-effort second-channel alert), AD-9 (the email and captcha ports already scaffolded by epic 1 — one adapter each, captcha verified Django-side; this epic picks both vendors and adds no ports), AD-10 (extends the revalidation path map for the contact page); convention: this epic owns the migration seeding `CONTACT_SETTINGS` (single row) and its read — env vars hold secrets only, never the contact target

## Notes

- Waits on epic-home-trust because: the form section closes the scroll that epic builds.
- Open question: Q3 — which email service delivers form messages and booking notifications; owner: this epic; blocks R5.
- Assumption: this epic's revalidation path-map extensions (AD-10) are additive, keyed `section:<key>` entries that no other epic writes, so no ordering against epic 3 is needed; a merge collision, should one appear, is resolved at merge time.
