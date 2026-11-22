---
title: "PRD — Software Engineer Portfolio"
template: prd
version: 1.1.0
status: final
created: 2026-11-22
updated: 2026-11-22
---

# PRD — Software Engineer Portfolio

## Table of Contents

- [Document purpose](#document-purpose) — who reads this and what it builds on
- [Vision](#vision) — the trust ladder walked by scroll; the honest-presence bet
- [Users and journeys](#users-and-journeys) — UJ-1 Mariam's booking scroll, UJ-2 Lina's code check, UJ-3 Omar's message (UJ-1, UJ-2, UJ-3)
- [Glossary](#glossary) — client, discovery call, project, scale figure, translation line, admin area
- [Features and requirements](#features-and-requirements) — seven features, FR-1 to FR-15
- [Rules](#rules) — RULE-1 to RULE-3: truthful stats, no fabricated trust, free call
- [Non-goals](#non-goals) — outreach, reviews, the call, money, CMS, blog
- [MVP scope](#mvp-scope) — what version 1 carries and what waits
- [Success metrics](#success-metrics) — SM-1 to SM-3: conversion, client rate, reply speed
- [Quality and constraints](#quality-and-constraints) — RULE-4 to RULE-7: speed, delivery, guard, cost
- [Experience and platform](#experience-and-platform) — tone, page shape, mobile-first
- [Integrations and operations](#integrations-and-operations) — scheduler, email, hosting
- [Data and compliance](#data-and-compliance) — visitor personal data
- [Decisions](#decisions) — the calls made so far
- [Open questions](#open-questions) — OQ-1 to OQ-10

## Document purpose

Hamza's own reference for what the portfolio must do and how to judge it, plus the document the epic work builds from: `bmad-ux` designs from it, `bmad-architecture` reads beside it, and Sofia's epic PRDs cite its ids rather than restating them. Inputs it builds on instead of repeating: the brainstorm conclusions at `_bmad-output/initiative-portfolio/brainstorm-software-engineer-portfolio/.memlog.md` and the initiative spec at `_bmad-output/initiative-portfolio/initiative-portfolio.md` (R1–R7, C1–C3).

## Vision

A stranger with a software problem — a restaurant manager drowning in spreadsheets, a startup founder who needs a product shipped, a small business owner who was burned by the last freelancer — finds Hamza's site. They know nothing about him. Within one screen they see a real face and a claim: powerful, scalable apps. They scroll, and every scroll answers the next doubt instead of raising a new one: what he has built, what scale it ran at in words a business owner understands, the code itself on GitHub, his voice on video, the promise in writing — free fixes for 30 days.

By the bottom of the page, the decision is small: book a free 10–15-minute discovery call, or leave a message and get a fast reply. No reviews exist yet, and the site says so honestly — first clients get his best work, and their review goes here: the honest launch message. The bet of this product: for a starting freelancer, visible proof and honest presence convert better than a wall of credentials, and the site's job is to make hiring Hamza feel safe rather than impressive.

## Users and journeys

- **UJ-1. Mariam, restaurant manager, decides to book the meeting by the end of one scroll session.** Mariam manages a restaurant drowning in manual bookings and has been quoted an arm and a leg by an agency; a friend shares Hamza's site. Beat 1 — the first screen: a real photo, the claim "builds powerful, scalable apps", and a presence cue; the instant check "is this person real and reachable?" passes. Beat 2 — the proof scroll: project tiles open project pages; each scale figure (users handled, requests per second) is translated into plain language — what it means for *her* busiest hours. Beat 3 — the depth scroll: a public GitHub link on the project page, an intro video with Hamza's voice, and the 30-day free-fix guarantee stated in writing. Beat 4 — the reviews section shows the honest launch message. Climax — the Book Now CTA opens a calendar of free 10–15-minute slots and Mariam books one in under a minute. Resolution — instant confirmation; she shows up to the call knowing what Hamza builds. Failure path: a flood of fake bookings is stopped by the spam guard before it can take the calendar hostage. Realized by FR-1, FR-2, FR-3, FR-4, FR-5, FR-6, FR-9, FR-10, FR-13.

- **UJ-2. Lina, CTO of a 12-person startup, judges the code without the small talk.** Lina is hiring a contractor for a backend build and has been burned by a portfolio of screenshots; she lands cold. She skips the hero and jumps to a project page close to her problem. She reads the scale figures with their plain-language lines — treating them as claims she can check — and clicks through to the public GitHub repo: real commits and clean, maintainable code. She leaves knowing whether Hamza can code, without a call. Edge case: a project built under client confidentiality shows its stats without a repo link, and the page says why. Realized by FR-2, FR-3, FR-4.

- **UJ-3. Omar, small business owner, isn't ready for a call but leaves a message.** Omar runs a hardware shop; he read the site on his phone during a slow afternoon, liked the guarantee, but won't book a meeting with a stranger. He opens the contact form, writes three sentences about his problem, and submits. The form confirms instantly on screen — thank you, you'll hear back soon — and the message lands in Hamza's email. Two hours later, Hamza replies fast, and the reply turns into a booked call. Failure path: a submit that Hamza never receives would burn exactly the trust the page built, so delivery, not just the confirmation screen, is the promise the form must keep. Realized by FR-7, FR-8, FR-10.

## Glossary

- **Client** — a visitor to the site who can become a paying customer of Hamza's development work; every journey below is one of theirs.
- **Discovery call** — the free 10–15-minute meeting a client books through the site; the conversion event this product exists to cause.
- **Project** — one piece of Hamza's work presented in the gallery; a project has a page, one or more scale figures, and optionally a public GitHub repo.
- **Scale figure** — a truthful number about a project, such as users handled, requests per second, or throughput.
- **Translation line** — the plain-language sentence paired with every scale figure, saying what the number means for a client's real situation.
- **Presence cue** — evidence on the first screen, beyond the photo and the positioning claim, that Hamza is real and reachable now: intro video, fast-reply note, or video-meeting offer.
- **Spam guard** — the mechanism that stops the booking calendar and the contact form from being flooded with fake submissions.
- **Guarantee** — the written promise of free fixes for 30 days after delivery.
- **Honest launch message** — the text the reviews section shows while no client reviews exist: first clients get my best work — and their review here.
- **Admin area** — the small private area where Hamza manages projects, the contact form's target info, and the sections' text content, without touching code.

## Features and requirements

**First-screen trust** — the hero answers "is this person real and reachable?" in one screen. Realizes UJ-1 beat 1.

- FR-1: A visitor sees, within the first screen, Hamza's real photo, the positioning claim (powerful, scalable apps), and at least one presence cue.
- FR-13: The home page carries Hamza's intro video — him speaking, voice audible — before the conversion point, so the voice the journeys promise is a requirement, not an option. His spoken English on the video is itself the proof of the English-speaking audience the MVP assumes. [added on review; carries initiative C1's video-presence trust carrier]

**Project proof pages** — the gallery and its project pages turn claims into checkable evidence. Realizes UJ-1 beats 2–3, UJ-2.

- FR-2: A visitor can open a project page from a gallery tile.
- FR-3: Every scale figure on a project page is paired with a translation line in plain language.
- FR-4: A project page presents its evidence — scale figures with translation lines, the technologies behind the system, the live URL when one exists, and screenshots — and links to the public GitHub repo when the code is publishable; when it is not (client confidentiality), the page states why.
- FR-14: The gallery launches with a real project — at least one, carrying at least one truthful scale figure with its translation line — and grows as Hamza adds new projects through the admin area after launch. An empty gallery kills the scroll before the proof starts; the reviews section's honesty (FR-9) must not be the only honest answer on the page. Client reviews are the primary trust carrier for the non-technical personas — Mariam and Omar — so until real reviews exist, real project proof stands in for them.

**Booking a discovery call** — the conversion feature. Realizes UJ-1 climax, failure path.

- FR-5: A visitor can book a free 10–15-minute discovery call from a calendar of open slots through the Book Now CTA, and sees an instant on-screen confirmation of the booking.
- FR-6: The booking path includes a spam guard that keeps the calendar free of floods of fake bookings — demonstrably: a scripted burst of fake bookings is blocked while a genuine booking completes.

**Contact form** — the fallback conversion for visitors not ready to book. Realizes UJ-3.

- FR-7: A visitor can submit a contact form whose message is delivered to Hamza's email.
- FR-8: On submit, the visitor sees an instant on-screen confirmation.

**Honest reviews section** — visible trust, honestly empty at launch. Realizes UJ-1 beat 4.

- FR-9: The reviews section is visible at launch and shows the honest launch message. When real reviews arrive, they carry what working with Hamza is like — process trust, not only outcomes.

**Delivery guarantee** — the written safety net for the fear of paying and being burned. Realizes UJ-1 beat 3, UJ-3.

- FR-10: The site states the guarantee: free fixes for 30 days after delivery.

**Content administration** — the operator side: Hamza keeps the proof current without code, so every journey meets fresh evidence. Serves UJ-1, UJ-2 and UJ-3 indirectly; a journey meets it only through the public features it feeds.

- FR-11: Hamza can add, edit, and remove projects through an admin area — including each project's scale figures, translation lines, technologies, live URL, screenshots, and repo link.
- FR-12: Hamza can manage the contact form's target info — the destination email address submitted messages are delivered to — and update the text content of the site's sections through the admin area, without touching code.
- FR-15: Only Hamza can access the admin area, through authentication; he is its only user. [added on review; the mechanism is the opening epic's]

## Rules

- RULE-1: Every scale figure shown on the site is truthful and carries a translation line; a number without its plain-language meaning does not ship.
- RULE-2: Nothing on the site fabricates trust — no invented reviews, no fake proof; the reviews section shows real reviews or the honest launch message, never both at once.
- RULE-3: Booking a discovery call costs the client nothing and asks no commitment beyond the call itself.

## Non-goals

- **Founding-client outreach.** Discounted engagements traded for honest reviews and showcase rights are a business process outside the site; the site only displays the honest launch message.
- **Real review content.** Nothing exists yet, and nothing below manufactures testimonials or placeholders dressed as reviews.
- **The discovery call itself.** The site books it; the call happens off-site. Nothing below adds meeting tooling, notes or follow-up automation.
- **Money on the site.** No payments, invoicing, client portal or contracts — the site converts, the engagement runs elsewhere.
- **A general-purpose CMS.** The admin area manages this site's projects, contact info and section text — no plugin ecosystem, no user roles beyond Hamza.
- **A blog or article section.** Proof lives in project pages, not articles; content marketing is a future version's question.

## MVP scope

Version 1 ships every requirement in Features and requirements, plus basic traffic analytics so the success metrics are readable. Everything else out of scope: see Non-goals.

- Out of version 1: multi-language versions of the site [ASSUMPTION: the audience is English-speaking; correct me if clients are expected in another language].

**Leanness note:** the admin area is the heaviest single piece in this scope, and the site could launch with projects managed in code and the admin added right after. You chose the admin deliberately, so it stays — but if the build overruns, FR-12's section-text editing is the first thing to defer to a 1.1 (see OQ-7). Nothing here is a placeholder: every in-scope item ships whole.

## Success metrics

- SM-1: Visitor-to-conversion — the share of visitors who book a discovery call or send the contact form. Current: none (no site). Target: at least 1 conversion per 100 unique visitors [ASSUMPTION: my draft target — set your own]. Validates FR-5, FR-7, FR-8, and with them the whole trust ladder. Counter-metric: first-screen bounce rate — if most visitors bounce and few convert, the ladder leaks before the proof.
- SM-2: Call-to-client — the share of discovery calls that become paying clients. Target: at least 1 in 3 [ASSUMPTION: my draft target]. Validates that the site sets honest expectations, so the call starts warm. Counter-metric: booked calls that no-show — a rising no-show rate says the spam guard is letting fakes through (FR-6).
- SM-3: Reply speed — Hamza answers form messages and booking requests within 24 hours. Target: 100% within 24h [ASSUMPTION: my draft target]. Validates FR-7, FR-8 and the fast-reply promise. Counter-metric: form messages that need a second exchange before a call — a sign the form should ask better questions.

## Quality and constraints

- RULE-4: The first screen renders fast, because the 3-second trust window is a budget: first meaningful paint on a mid-range phone under 2 seconds [ASSUMPTION: threshold is my pick]. Protects FR-1; its felt-side check is SM-1's bounce counter-metric — speed alone doesn't make a visitor feel trust.
- RULE-5: A submitted contact message and a completed booking each either reach Hamza — the form as email, the booking as a notification — or the failure is surfaced to him through a second channel; a silently lost submission counts as a failed submit. Protects FR-5, FR-7, FR-8. [from UJ-3's failure path, extended to the primary conversion by review]
- RULE-6: The spam guard stops floods without stopping real visitors — a genuine booking or message completes with no more than the guard's single check [ASSUMPTION: trade-off stated so the guard is tuned against false positives]. Protects FR-6.
- RULE-7: Running cost stays at a small fixed monthly amount a starting freelancer pays from one client's invoice [ASSUMPTION: no ceiling was given — name one if you have it]. Protects every FR.

## Experience and platform

- **Tone:** plain, honest, confident; every technical claim carries its human meaning — the translation-line habit applies to the site's own copy, not only to scale figures.
- **Page shape:** one scroll from hero to reviews and guarantee; Book Now visible from the first screen and repeated after the proof; the contact form waits at the end as the fallback.
- **Platforms:** mobile-first — UJ-3 happens on a phone during a slow afternoon — with an equivalent desktop experience; no native app.
- **Monetization:** none on the site (Non-goals).

## Integrations and operations

The site leans on three outside services, each owned by an epic:

- **Scheduling (FR-5, FR-6):** a third-party scheduler — decided 2026-11-22 (OQ-1); it beat a custom calendar on build speed and carries most of the spam protection.
- **Email delivery (FR-7):** the service that carries form messages and booking notifications — unsettled, OQ-3.
- **Hosting (every FR):** platform and stack — deferred to after MVP planning by the owner (OQ-4); must settle before the opening epic.

Service levels: the booking path and the form are the conversion path — downtime there is lost clients, so their availability is a build concern whose threshold the architecture sets. RULE-5 already names the form's delivery guarantee.

## Data and compliance

Personal data collected: name, email address, and the message itself, used only to reply to the sender and, for a booking, to hold the slot. No phone number is asked — the captcha guards the forms instead (decided 2026-11-22). Retention and hosting region are not settled — OQ-5. If clients come from the EU, their data rules apply and the site must say plainly what happens to submitted data [ASSUMPTION: client geography unknown — the answer decides whether this grows].

## Decisions

- Template `prd` over `prd-light` — real stakes; freelance income rides on this site's conversion.
- Journey-led structure — the product is a trust ladder walked by scroll; your brain dump was itself a journey.
- Content administration in scope (2026-11-22) — a small admin area manages projects, contact-form info and section text; it beat developer-managed static content.
- No-repo projects still prove themselves (2026-11-22) — FR-4: projects without a publishable repo show their evidence and state why; it beat showing nothing.
- No blog (2026-11-22) — accepted as a non-goal on proposal.
- Admin edits content, never layout (2026-11-22) — deferred to `bmad-architecture` rather than written as a rule.

## Open questions

- OQ-1: Decided 2026-11-22 — a third-party scheduler; see Decisions. [was: custom calendar or third-party]
- OQ-2: Decided 2026-11-22 — captcha; see Decisions. [was: phone verification, captcha, or confirmation step]
- OQ-3: Which email service delivers form messages and booking notifications? Owner: the contact-form epic; blocks FR-7. [from initiative Q3]
- OQ-4: Deferred 2026-11-22 by the owner — hosting platform and stack are discussed after MVP planning; must settle before the opening epic. [from initiative Q4]
- OQ-5: Data retention and hosting region for visitor personal data; whether EU clients are in the audience. Owner: Hamza; needed before launch, blocks nothing in the build.
- OQ-6: Partly resolved 2026-11-22 — SM-2's one-in-three target confirmed; SM-1 and SM-3's targets still to confirm. Owner: Hamza; needed to judge the site after launch.
- OQ-7: The leanness fallback in MVP scope — confirm at MVP planning whether it triggers and who calls it. Owner: Hamza; a fallback, not a decision yet.
- OQ-8: Resolved 2026-11-22 — one real project at launch; more are built after the website launches and added through the admin area (FR-11, FR-14). [was: which real projects exist and are presentable at launch, and how many]
- OQ-9: The running-cost ceiling for RULE-7 — the number, so the rule becomes testable. Owner: Hamza; needed at the hosting decision (OQ-4). [from review]
- OQ-10: Resolved 2026-11-22 — the guarantee's detailed terms live in each client's contract after a deal, not on the site; the site copy states only the 30-day free-fix promise (FR-10). [was: what counts as a fix and when the 30 days start]
