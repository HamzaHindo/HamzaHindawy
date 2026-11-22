---
id: 4
type: epic
title: "Contact form and email delivery"
parent: initiative-portfolio
covers: ["R5"]
sources: []
companions: []
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

## Notes

- Waits on epic-home-trust because: the form section closes the scroll that epic builds.
- Open question: Q3 — which email service delivers form messages and booking notifications; owner: this epic; blocks R5.
