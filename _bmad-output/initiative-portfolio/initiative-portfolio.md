---
type: initiative
title: "Software Engineer Portfolio"
parent: none
sources:
  - _bmad-output/initiative-portfolio/brainstorm-software-engineer-portfolio/.memlog.md
  - _bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md
companions: []
after: []
risk: medium
---

# Software Engineer Portfolio

## Description

A personal portfolio website for Hamza, a freelance software engineer, built to convert visitors into paying clients. It presents trust before technology: a real photo and positioning message on the first screen, clickable project pages with real scale statistics translated into plain language and backed by public GitHub repos, a free discovery-call booking flow protected against spam, and a contact form fallback — launched honestly with no client reviews yet and carried by a 30-day free-fix guarantee. A small admin area keeps the proof current without touching code. Derived from the brainstorm at `_bmad-output/initiative-portfolio/brainstorm-software-engineer-portfolio/.memlog.md` and specified in full by the PRD at `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`.

## Outcome

Freelance prospects visiting the site trust Hamza enough to book a free discovery call or send a contact message; the brainstorm's own measure of success is converting visitors into paying clients. After launch, Hamza keeps the site's proof current through the admin area, without code.

## Requirements

- R1 (FR-1): Within the first screen, a visitor sees Hamza's real photo, the positioning claim that he builds powerful, scalable apps, and at least one presence cue that he is real and reachable now.
- R2 (FR-2, FR-3): A visitor opens a project page from a gallery tile, and every scale figure on a project page is paired with a translation line in plain language.
- R3 (FR-4): A project page presents its evidence — scale figures with translation lines, the technologies behind the system, the live URL when one exists, screenshots — and links to the public GitHub repo when the code is publishable; when it is not, the page states why.
- R4 (FR-5, FR-6): A visitor can book a free 10–15-minute discovery call from a calendar of open slots through the Book Now CTA and sees an instant on-screen confirmation; the booking path includes a spam guard, demonstrably: a scripted burst of fake bookings is blocked while a genuine booking completes.
- R5 (FR-7, FR-8): A visitor can submit a contact form whose message is delivered to Hamza's email, and sees an instant on-screen confirmation on submit.
- R6 (FR-9): The reviews section is visible at launch and shows the honest launch message; when real reviews arrive they carry what working with Hamza is like — process trust, not only outcomes.
- R7 (FR-10): The site states the guarantee in writing: free fixes for 30 days after delivery.
- R8 (FR-13): The home page carries Hamza's intro video — him speaking, voice audible — before the conversion point.
- R9 (FR-14): The gallery launches with at least one real project carrying at least one truthful scale figure with its translation line, and grows as Hamza adds new projects through the admin area after launch.
- R10 (FR-11): Hamza can add, edit, and remove projects through the admin area — each project's scale figures, translation lines, technologies, live URL, screenshots, and repo link — without touching code.
- R11 (FR-12): Hamza can manage the contact form's target email and update the text content of the site's sections through the admin area, without touching code.
- R12 (FR-15): Only Hamza can access the admin area, through authentication; he is its only user.
- R13 (MVP scope, SM-1–SM-3): Basic traffic analytics ships in version 1 so the success metrics — visitor-to-conversion, call-to-client, reply speed — are readable.

## Constraints

- C1 (RULE-2, brainstorm): The site carries the trust load without real client reviews at launch; the guarantee, the free call and the video presence carry the trust transfer instead.
- C2 (RULE-3): Booked discovery calls are free and short (10–15 minutes) and ask no commitment beyond the call itself; they are the conversion step the funnel is built around.
- C3 (RULE-1): Every scale figure shown is truthful and carries its translation line; a number without its plain-language meaning does not ship.
- C4 (RULE-2): Nothing on the site fabricates trust — no invented reviews, no fake proof; the reviews section shows real reviews or the honest launch message, never both at once.
- C5 (RULE-4): The first screen renders fast: first meaningful paint on a mid-range phone under 2 seconds.
- C6 (RULE-5): A submitted contact message and a completed booking each reach Hamza — the form as email, the booking as a notification — or the failure is surfaced to him through a second channel; a silently lost submission counts as a failed submit.
- C7 (RULE-6): The spam guard stops floods without stopping real visitors: a genuine booking or message completes with no more than the guard's single check.
- C8 (RULE-7): Running cost stays at a small fixed monthly amount a starting freelancer pays from one client's invoice.
- C9 (Experience and platform): Mobile-first — UJ-3 happens on a phone — with an equivalent desktop experience; no native app.

## Done when

- A visitor landing on the home page sees photo, positioning claim and a presence cue within the first screen, and the intro video sits before the conversion point.
- A visitor can walk tile → project page → scale figures with plain-language lines → GitHub repo link, and the launch gallery carries at least one real project.
- A visitor can book a free discovery slot and see instant confirmation, and the spam guard demonstrably blocks a flooding attempt while a genuine booking completes.
- A visitor can submit the contact form, see instant confirmation, and the message arrives in Hamza's email.
- The reviews section shows the honest launch message, and the 30-day guarantee is stated on the site.
- Hamza can add, edit and remove a project, and change the contact target and section text, through the authenticated admin area without touching code.

## Boundaries

- The initiative is the public portfolio website itself; greenfield — no existing site or codebase to change in this repo.
- Out: founding-client outreach (discounted engagements traded for honest reviews and showcase rights) — a business process outside the site; the site only displays the honest launch message.
- Out: real client review content — nothing exists yet to display, and nothing manufactures testimonials.
- Out: the discovery call itself and any meeting tooling, notes or follow-up automation — the site books it, it happens off-site.
- Out: money on the site — no payments, invoicing, client portal or contracts.
- Out: a general-purpose CMS — the admin area manages this site's projects, contact info and section text only; no plugin ecosystem, no roles beyond Hamza.
- Out: a blog or article section — proof lives in project pages.
- Out: multi-language versions of the site in version 1 (A2).
- Touch point: Hamza's email inbox — form messages and booking notifications arrive there; owner: the booking/contact epics.
- Touch point: the third-party scheduler — owns open slots and most of the spam protection; owner: the booking epic.
- Touch point: the email delivery service — owner: the contact-form epic (Q3).
- Tracer path: land → trust cues → project page with stats and GitHub link → book a discovery call or submit the contact form.

## References

- source — `_bmad-output/initiative-portfolio/brainstorm-software-engineer-portfolio/.memlog.md`, brainstorm decisions, ideas and insights
- source — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, the final PRD: journeys UJ-1 to UJ-3, features FR-1 to FR-15, rules RULE-1 to RULE-7, success metrics SM-1 to SM-3, integrations and decisions; cited by id rather than restated

## Notes

- Assumption: A1 — the portfolio is a new greenfield build in this repo; no existing site, hosting, domain or stack is in place.
- Assumption: A2 — the audience is English-speaking; multi-language versions are out of version 1.
- Open question: Q3 — which email service delivers form messages and booking notifications; owner: the contact-form epic; blocks FR-7.
- Open question: Q4 — hosting platform and stack, deferred 2026-11-22 by the owner to after MVP planning; must settle before the opening epic.
- Open question: Q5 — data retention and hosting region for visitor personal data, and whether EU clients are in the audience; owner: Hamza; needed before launch, blocks nothing in the build.
- Open question: Q6 — SM-2's one-in-three target confirmed; SM-1 and SM-3's draft targets still to confirm; owner: Hamza.
- Open question: Q7 — whether the leanness fallback triggers (R11's section-text editing deferred to a 1.1) and who calls it; owner: Hamza; a fallback, not a decision yet.
- Open question: Q9 — the running-cost ceiling for RULE-7, so the rule becomes testable; owner: Hamza; needed at the hosting decision.
- Decision: 2026-11-22 — a third-party scheduler for booking (Q1 resolved); it beat a custom calendar on build speed and carries most of the spam protection.
- Decision: 2026-11-22 — captcha guards the booking and the form; no phone number is asked (Q2 resolved).
- Decision: 2026-11-22 — one real project at launch; more are built after launch and added through the admin area.
- Decision: 2026-11-22 — the guarantee's detailed terms live in each client's contract after a deal; site copy states only the 30-day free-fix promise.
- Decision: 2026-11-22 — content administration in scope; the admin area beat developer-managed static content.
- Decision: 2026-11-22 — no-repo projects still prove themselves: they show their evidence and state why (R3).
- Decision: 2026-11-22 — one email service carries form messages and booking notifications; the booking epic waits on the contact epic's choice (Q3). Owner call on the epic split.
- Decision: 2026-11-22 — the hosting platform is settled after the website is built, overriding PRD OQ-4's before-the-opening-epic timing; a launch epic (epic 7) owns hosting, domain, the cost ceiling and production verification; the build stack is still picked in the foundation epic's first story.
- Decision: 2026-11-22 — sliced into seven epics in build order: foundation → home trust → project proof → contact form + email → booking → admin → launch. The captcha's home is the contact epic (earliest needer); R9 splits launch part (epic 3) from growth part (epic 6); declined: transitive after entries — prerequisites are declared once, at the point of need.
