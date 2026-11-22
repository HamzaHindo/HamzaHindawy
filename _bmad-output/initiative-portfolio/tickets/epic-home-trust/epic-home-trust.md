---
id: 2
type: epic
title: "First-screen trust and the honest scroll"
parent: initiative-portfolio
covers: ["R1", "R6", "R7", "R8"]
sources: []
companions: ["_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md"]
after: ["epic-portfolio-foundation"]
refined: false
risk: low
---

# First-screen trust and the honest scroll

## Description

The trust ladder's first rungs: a cold visitor's first screen shows Hamza's real photo, the positioning claim and a presence cue, the intro video plays with his voice audible before the conversion point, and the honest reviews message and the 30-day guarantee sit in the scroll. Provides the hooks later epics build into: the home page shell with its gallery slot, the hero with a Book Now CTA slot, and the scroll the contact section closes.

## Outcome

A cold visitor passes the is-he-real-and-reachable check in one screen and reaches the bottom of the scroll knowing the guarantee and the honest state of reviews.

## Done when

- The first screen shows photo, positioning claim and at least one presence cue, checked on a phone-sized viewport (production timing comes at epic 7).
- The intro video plays with audible voice before the conversion point.
- The reviews section is visible and shows the honest launch message.
- The guarantee — free fixes for 30 days after delivery — is stated in the scroll.

## Boundaries

- Belongs to siblings: gallery tiles and project pages (epic 3), the CTA's calendar (epic 5), the contact form (epic 4), editing this section text without code (epic 6), production speed (epic 7).
- Touch point: the intro video asset — Hamza records it; the epic gives it a place and a player.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Requirements R1, R6, R7, R8
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, FR-1, FR-9, FR-10, FR-13, UJ-1
- companion — `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md`, spine for this epic: AD-4, AD-5, AD-10 (extends the revalidation path map for the pages it adds), AD-11 (consumes the shared SSR fetch layer; the intro video never blocks first paint); convention: this epic creates and owns the single section-key registry — the honest-launch message and the guarantee copy are `content` rows seeded here, never hardcoded in `web/`

## Notes

- Waits on epic-portfolio-foundation because: the pages it builds must run on the scaffold with analytics.
- Open question: the video's content and length — Hamza records it; settles at refinement.
