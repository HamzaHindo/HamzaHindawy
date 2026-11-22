---
type: review
review: reconciliation
reviewed: architecture-spine 'Software Engineer Portfolio' (draft, 2026-11-22)
against:
  - _bmad-output/initiative-portfolio/initiative-portfolio.md
  - _bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md
verdict: drift found
created: 2026-11-22
---

# Reconciliation Review — Architecture Spine vs Initiative + PRD

**Verdict: drift found.** The spine's ADs are internally coherent and several are genuinely good (AD-9's one-adapter-per-vendor, the scale-figure triple encoding RULE-1 by construction, AD-10's freshness contract). But the document drifted from its sources in five serious ways: the Capability → Architecture Map's requirement IDs are mislabeled so badly that the checklist itself is broken; a hard quality budget (RULE-4) is dropped; the spine claims an open question settled that the owner still owns; an invented feature was quietly promoted into the mechanism for a real rule; and the compliance half of OQ-5 was silently discarded.

---

## F1 — The Capability → Architecture Map's R-IDs do not match the initiative's R-IDs (the checklist itself is broken)

The initiative defines R1–R13 explicitly (R1 = FR-1 first screen … R4 = FR-5/FR-6 booking, R5 = FR-7/FR-8 contact form, R6 = FR-9 reviews, R7 = FR-10 guarantee, R8 = FR-13 video, R9 = FR-14 launch gallery, R10 = FR-11 admin projects, R11 = FR-12 contact target + section text, R12 = FR-15 auth, R13 = analytics).

The spine's map (lines 177–185) uses different assignments:

| Map row says | Initiative actually says | Correct ID |
| --- | --- | --- |
| "R4, FR-4 (evidence, repo link / no-repo reason)" | R4 = **booking + spam guard**; evidence/repo is R3 | R3 (FR-4) |
| "R5 (booking + spam guard)" | R5 = **contact form + confirmation**; booking is R4 | R4 (FR-5, FR-6) |
| "R6, R5-contact (contact form, email delivery)" | R6 = **reviews section**; contact form is R5 | R5 (FR-7, FR-8) |
| "R7 (reviews, honest launch message)" | R7 = **30-day guarantee**; reviews is R6 | R6 (FR-9) |
| "R10 (30-day guarantee in writing)" | R10 = **admin project CRUD**; guarantee is R7 | R7 (FR-10) |
| "R11, R12, FR-15 (admin area)" | R11 = contact target + section text; R12 = auth only; **admin project CRUD is R10** | R10, R11, R12 |
| — | **R9 (gallery launches with ≥1 real project with a truthful scale figure) appears nowhere in the spine body** — only in the frontmatter `binds` list | R9 (FR-14) |

The rows from R4 onward are shifted/duplicated as if the author half-mapped R-n to FR-n. Consequence: **R9 is silently ungoverned** — and R9 is not cosmetic; it is the launch-content requirement that keeps the gallery from being empty at launch (PRD FR-14: "an empty gallery kills the scroll before the proof starts"). Nothing in the spine assigns launch content ownership to any epic. Every downstream reader who uses this map as the checklist (per the review protocol) inherits the wrong bindings.

**Fix:** re-key the map to the initiative's R-IDs exactly, add an R9 row (launch seed content — likely the launch epic or a seed fixture in the foundation epic), and state in each row which FR ids the row covers.

## F2 — RULE-4's 2-second budget is dropped, and the chosen topology actively threatens it

RULE-4 / C5: *first meaningful paint on a mid-range phone under 2 seconds*. The map row for R1/R8 says "RULE-4 via hosting constraint" — but **the budget is never stated anywhere in the spine**: no AD, no convention, no Deferred row carries the 2-second number or the mid-range-phone target. "Hosting constraint" is a gesture, not a governance.

Worse, the topology chosen makes the budget *fragile*: public pages are server-rendered by Next.js (AD-5, AD-10), and every SSR render depends on a Django API that the spine itself admits "must stay warm" (Structural Seed). A cold Python host on a free/cheap tier blows the 2-second budget on exactly the first screen that must win the 3-second trust window. The spine names the warm-API constraint parenthetically and assigns it to epic 7, but no AD governs it (no invariant about cold-start behavior, no caching/edge-cache rule for public reads, no "first paint must not depend on a cold API call" rule).

**Fix:** state the RULE-4 budget explicitly as a bound invariant; add an AD (or extend AD-10) making first-paint independent of API cold starts (e.g., ISR/edge-cached public pages, budget-check in the launch epic's production verification).

## F3 — The spine settles what the owner still owns: RULE-7's ceiling "already fixed here" (Q9/OQ-9), and Vercel pre-empts the hosting decision (Q4/OQ-4)

- The Deferred table says: *"the warm-API constraint (AD-5 note) and RULE-7 ceiling are already fixed here."* **The ceiling is not fixed anywhere in the spine** — no number appears. Q9/OQ-9 explicitly says the running-cost ceiling is unset, owner Hamza, needed at the hosting decision. The spine asserts settlement of an open question its own sources mark open. (AD-1's "doubled hosting cost against RULE-7" cites a rule it never quantifies.)
- AD-5 and the Stack table commit the frontend to **Vercel (Hobby plan)**. The initiative's 2026-11-22 decision: hosting platform is settled *after* the build, owned by launch epic 7 ("hosting, domain, the cost ceiling and production verification"). Deferring the *Python host* to epic 7 while fixing the *frontend host* in the spine splits one owner decision across two documents. If the intent is "Vercel is the default unless epic 7 decides otherwise," the spine must say so; as written it pre-empts Q4.
- AD-5 claims the stack was *"Chosen with tradeoffs accepted by the owner (learning React; two deployables; SSR/SEO quality over single-server simplicity)."* **No source records the owner accepting these tradeoffs** — the ticket and PRD contain no stack decision at all (the build stack is "still picked in the foundation epic's first story"). If the owner did accept, the acceptance lives nowhere; the spine should cite it or mark the stack provisional pending the foundation epic.

**Fix:** mark the RULE-7 ceiling as open with owner Hamza (link OQ-9), reword Vercel as provisional/default pending epic 7, and either source or soften the "accepted by the owner" claim.

## F4 — AD-3 invents the messages inbox, then AD-7 makes that invention the sole mechanism for RULE-5 — which it may not satisfy

AD-3 (read-only admin messages inbox) is honestly flagged as *"deliberate scope addition beyond FR-11/FR-12 — flagged for a ticket update via Sofia."* Two problems survive the flag:

1. **The flag is not a change.** Until Sofia actually updates the ticket, the inbox is unsanctioned scope — and it is not optional garnish: AD-7 makes it *the* second channel of RULE-5. A load-bearing mechanism rests on an unaccepted scope addition.
2. **A passive inbox arguably fails RULE-5.** C6/RULE-5: a lost submission's failure must be *"surfaced to him through a second channel."* AD-7's mechanism is a status field in an inbox Hamza must log in and look at; booking-notification failure is only *"checkable in the scheduler dashboard."* "He can discover the failure if he goes looking" is weaker than "the failure is surfaced to him." AD-7's own *Prevents* line rules out "an unprompted second alerting service" — but the rule it implements may be what requires prompting. At minimum the spine should state why a passive surface is judged sufficient, or route failures through a channel Hamza already reads (e.g., a fallback email to a second address — the email port already exists per AD-9).

Also note the wider invention AD-2 makes on the same footing: **store-and-forward of contact messages in the DB** is not asked for by FR-7 (delivery to email is the requirement; storage is the invention that enables AD-3/AD-7). It is defensible, but it enlarges the personal-data footprint (see F5) and it is the same unsanctioned scope.

## F5 — OQ-5's compliance half is silently dropped; C9, RULE-3, and FR-4's data model also fall through

- **OQ-5 (Q5) is only half carried.** The Deferred table covers retention and hosting region. What the spine drops entirely: *"whether EU clients are in the audience"* and the PRD's conditional — *"their data rules apply and the site must say plainly what happens to submitted data."* No AD touches consent/privacy notice, and the personal-data surface is larger than the spine admits: messages are now stored in the DB (AD-2), the captcha and scheduler vendors process visitor data, and object storage holds screenshots (not personal, but vendor-resident). The data-protection posture is a gap the spine should at least name, not omit.
- **C9 (mobile-first, equivalent desktop, no native app) appears nowhere.** Quietly dropped. For a front-end architecture this deserves one line at minimum (responsive SSR pages; no native app).
- **RULE-3 (free 10–15-min call, no commitment) appears nowhere.** Mostly product copy, but the booking path is architected (scheduler port, AD-9) and the free-call promise is the funnel's conversion step — one line binding it would cost nothing.
- **FR-4's data model is incomplete.** The ER diagram and `projects` module listing name scale figures, technologies, screenshots — but not the **repo link, the live URL, or the no-repo "reason"** the page must state (FR-4/R3, and the conventions table's identity rule mentions none of them). The one place "repo link / no-repo reason" appears is the mislabeled F1 map row. As drawn, the data model cannot represent a confidential project's explanation.
- **R13 is governed too thinly.** "Vercel Web Analytics (cookieless; keeps R13)" — but R13 exists so SM-1–SM-3 are readable. SM-1 needs **conversion events** (booking completed, form submitted), not pageviews; SM-3's 24-hour reply speed isn't an analytics question at all. The spine should state that conversion tracking is in the analytics scope or flag what SM-1 will actually be read from.

## Minor findings

- **AD-6's `SameSite=None` cross-origin session cookie** is a security-posture downgrade chosen to serve the two-deployable split (Vercel UI ↔ separate API origin). Nothing in the sources forces the split-origin deployment; a co-located deployment would allow SameSite=Lax. The choice deserves its tradeoff stated, not just "Django CSRF protection" as a talisman.
- **Booking-notification assumption:** AD-7 assumes "the scheduler's own delivery" notifies Hamza of bookings. That is a vendor capability, not a given; the booking epic's port contract should require it explicitly, else RULE-5's booking half rests on an unchecked assumption.
- **ER diagram cardinalities are inverted/sloppy:** `SECTION_TEXT ||--o{ SECTION` and `CONTACT_SETTINGS ||--o{ MESSAGE` read backwards (a message has one target email; a section row is one section's text). Cosmetic but this diagram is the seed.
- **Tone (Experience: "the translation-line habit applies to the site's own copy")** is unenforceable architecturally and the spine is silent — acceptable, but the scale-figure triple covers only figures, not copy; note the boundary so nobody claims the schema enforces the tone.
- **Version pins** (Django 6.1.3, DRF 3.18.3, Next 16.3, Node 26 "Lithium", React 19.3) are invented specifics — legitimate spine work, but they should be marked provisional since the build stack is "picked in the foundation epic's first story" per the initiative's own decision.

---

## Checklist audit

### R1–R13 (initiative IDs)

| Req | Spine coverage | Verdict |
| --- | --- | --- |
| R1 first screen (photo, claim, presence cue) | map row (correct), AD-5/AD-10 | Covered |
| R2 tile → page; translation lines | map row, conventions triple | Covered |
| R3 evidence + repo link / no-repo reason | map row **mislabeled as "R4"**; data model omits repo/live URL/reason | **Drift (F1, F5)** |
| R4 booking + spam guard | map row **mislabeled as "R5"**; AD-2, AD-9, RULE-6 | Covered, mis-keyed (F1) |
| R5 contact form + instant confirmation | map row **mislabeled as "R6/R5-contact"**; AD-2, AD-7 | Covered, mis-keyed (F1) |
| R6 reviews, honest launch message | map row **mislabeled as "R7"**; content module | Covered, mis-keyed (F1) |
| R7 30-day guarantee in writing | map row **mislabeled as "R10"** | Covered, mis-keyed (F1) |
| R8 intro video before conversion point | map row with R1 | Covered (placement "before the conversion point" is UI detail — acceptable) |
| R9 launch gallery ≥1 real project | **Absent from spine body** | **Gap (F1)** |
| R10 admin project CRUD | map row **mislabeled as "R11/R12"**; AD-8, AD-10 | Covered, mis-keyed (F1) |
| R11 contact target + section text editing | lumped into "R11, R12, FR-15" row | Covered, mis-keyed (F1); OQ-7 contingency unnoted (F3-adjacent) |
| R12 admin auth, only Hamza | AD-6 | Covered (SameSite note, minor) |
| R13 basic analytics for SM-1–3 | stack row only | **Thin (F5)** |

### C1–C9

| C | Spine coverage | Verdict |
| --- | --- | --- |
| C1 trust without reviews (guarantee/call/video carry it) | implicit via R6/R7/R8 rows | Covered (relies on mis-keyed rows) |
| C2 free 10–15-min call, no commitment | **RULE-3 never appears** | **Dropped (F5)** |
| C3 truthful figures + translation lines | conventions (value, unit, translation_line) | Covered — the spine's best move |
| C4 nothing fabricated; real or honest message, never both | content module, RULE-2 in map | Covered |
| C5 first paint < 2s mid-range phone | referenced, never stated; topology endangers it | **Drift (F2)** |
| C6 delivery or surfaced failure | AD-7 | **Governs weakly (F4)** |
| C7 guard stops floods, single check for real visitors | conventions (captcha = single check) | Covered |
| C8 small fixed monthly cost | cited (AD-1, Deferred) but ceiling claimed settled while open | **Drift (F3)** |
| C9 mobile-first, no native app | **never mentioned** | **Dropped (F5)** |

### RULE-1–7

| Rule | Verdict |
| --- | --- |
| RULE-1 | Covered by construction (triple) — good |
| RULE-2 | Covered (content module) |
| RULE-3 | **Absent** |
| RULE-4 | Referenced but budget never stated; cold-start threat unmanaged — **drift** |
| RULE-5 | Covered by AD-7, but passivity may fail the rule — **governs weakly** |
| RULE-6 | Covered (single captcha check) |
| RULE-7 | Cited; ceiling falsely claimed fixed — **drift** |

### Open questions the ticket/PRD own

| Q | Spine treatment | Verdict |
| --- | --- | --- |
| Q3 / OQ-3 email service | Deferred to contact-form epic; port + status fixed | Correct |
| Q4 / OQ-4 hosting | Python host deferred to epic 7; **Vercel + stack pre-committed**; "owner accepted tradeoffs" unsourced | **Partially pre-empted (F3)** |
| Q5 / OQ-5 retention, region, EU | Retention + region deferred; **EU branch and plain-language data notice dropped** | **Half-dropped (F5)** |
| Q6 / OQ-6 SM targets | Not claimed by spine | Correct (untouched) |
| Q7 / OQ-7 leanness fallback | Deferred | Correct — but spine ignores that a triggered fallback changes AD-1/AD-10 (section text leaves the DB); one sentence acknowledging the architectural contingency would prevent a silent contradiction |
| Q9 / OQ-9 cost ceiling | **Claimed "already fixed here" with no number** | **Falsely settled (F3)** |
| OQ-8 (resolved: one real project at launch) | Related: R9 seed content ungoverned | Tie to F1 |
| OQ-10 (resolved: guarantee terms in contract) | Guarantee copy in content module only | Consistent |

### Things the spine invents (beyond sources)

1. **AD-3's messages inbox** — self-flagged, but load-bearing for AD-7/RULE-5 (F4).
2. **Store-and-forward message persistence** (AD-2) — enables 1, not requested by FR-7; enlarges personal-data surface (F4, F5).
3. **"Tradeoffs accepted by the owner"** for the stack, including the "learning React" rationale — no source records this (F3).
4. **Vercel Hobby as frontend host** — pre-empts epic 7's hosting ownership (F3).
5. **Vercel Web Analytics as R13's implementation** — defensible default, but unexamined against what SM-1 actually needs (F5).

---

## What did land well (for balance)

- RULE-1 encoded structurally (the scale-figure triple) rather than as a hope.
- AD-9's one-adapter-per-shared-vendor rule directly serves the initiative's "one email service" and "captcha's home is the contact epic" decisions.
- AD-10 solves a real two-deployable freshness problem the PRD's "fresh evidence" promise implies.
- The Deferred table's shape is right for Q3 and Q7 — the failures are in what it claims settled, not in the deferral pattern itself.

## Recommended actions

1. Re-key the Capability → Architecture Map to the initiative's R-IDs; add R9 (launch seed content) with an owning epic.
2. State RULE-4's 2-second budget as a bound invariant and add a cold-start/first-paint AD (or extend AD-10).
3. Correct the Deferred row: RULE-7 ceiling is open (OQ-9, owner Hamza); mark Vercel provisional pending epic 7; source or soften the owner-tradeoff claim.
4. Either get Sofia's ticket update for the inbox before it carries RULE-5, or re-point RULE-5's second channel at a push-style fallback (e.g., second-address email via the existing port); state the booking-notification vendor requirement in the scheduler port.
5. Add: EU/privacy-notice branch of OQ-5, C9 mobile-first line, RULE-3 line, repo-link/live-URL/no-repo-reason fields to the data model, and a conversion-event requirement under R13.
