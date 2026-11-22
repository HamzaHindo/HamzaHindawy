---
id: 5
type: epic
title: "Discovery-call booking"
parent: initiative-portfolio
covers: ["R4"]
sources: []
companions: []
after: ["epic-home-trust", "epic-contact-form"]
refined: false
risk: medium
---

# Discovery-call booking

## Description

The conversion feature: a visitor books a free 10–15-minute discovery call from a calendar of open slots through the Book Now CTA and sees instant on-screen confirmation. The spam guard is demonstrable — a scripted burst of fake bookings is blocked while a genuine booking completes with no more than the single check of the captcha guard epic 4 introduced. A completed booking reaches Hamza as a notification through the email service epic 4 settled, or the failure is surfaced through a second channel. The third-party scheduler is chosen here.

## Outcome

The visitor's decision becomes a booked call in under a minute, and the calendar cannot be taken hostage by fakes.

## Done when

- A genuine booking completes through the CTA with instant on-screen confirmation.
- A scripted burst of fake bookings is blocked while a genuine booking completes with no more than the guard's single check.
- A completed booking reaches Hamza (notification), or its failure is surfaced through a second channel.
- The scheduler decision is recorded, and the booking conversion event lands in analytics.

## Boundaries

- Belongs to siblings: the contact form and its guard introduction (epic 4), the CTA slot in the hero (epic 2), production availability (epic 7).
- Out: the call itself and any meeting tooling — the site books it, it happens off-site.
- Touch point: the third-party scheduler — chosen and owned here; owner: this epic.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Requirement R4
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, FR-5, FR-6, RULE-3, RULE-5, RULE-6, UJ-1

## Notes

- Waits on epic-home-trust because: the Book Now CTA sits in the hero that epic builds.
- Waits on epic-contact-form because: its booking notifications send through the email service that epic settles (owner call 2026-11-22: one email service for everything).
