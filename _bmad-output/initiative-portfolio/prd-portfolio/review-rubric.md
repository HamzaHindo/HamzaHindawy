---
topic: Rubric review — prd-portfolio.md
template: rubric-planning
reviewed: 2026-11-22
document: `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md` v1.1.0
inputs-checked:
  - `.agents/skills/bmad-facilitate/references/rubric-planning.md`
  - `_bmad-output/initiative-portfolio/initiative-portfolio.md` (R1–R7, C1–C3, Done-when, Boundaries)
  - `_bmad-output/initiative-portfolio/brainstorm-software-engineer-portfolio/.memlog.md`
  - `_bmad-output/initiative-portfolio/prd-portfolio/.memlog.md`
---

## Verdict

This is a decision-ready, earned PRD: one clear thesis (visible proof and honest presence beat a credential wall), journeys trace to FRs and back to the initiative's R1–R7 with only one real drop, assumptions are tagged inline where a facilitator guessed on the fast path, and the altitude discipline is genuinely good — screens, architecture and edge cases are consistently left to the epics. It falls short of shippable on four points where the document's own logic demands a rule or a checkable number it does not give: delivery of the booking itself, the confirmations both journeys promise, the video the parent constraint calls load-bearing, and two thresholds tagged "my draft" that no open question then owns.

## The three fixes that matter most

1. **Guarantee the booking reaches Hamza.** RULE-5 protects only the contact form; a booking that silently never reaches Hamza's inbox burns the conversion event the whole product exists to cause, and the initiative's Boundaries explicitly put *both* form messages and booking notifications at the email touch point. Extend RULE-5 (or add a sibling rule) to cover booking notifications.
2. **Give both promised confirmations an FR.** UJ-1's resolution promises instant confirmation after booking and UJ-3 promises it after form submit, but FR-8 covers only the form — the booking confirmation exists in no FR, so an epic could omit it, and unconfirmed bookings feed straight into SM-2's no-show counter-metric.
3. **Pin the intro video or state why C1 is demoted.** Initiative C1 names video presence as one of the three trust carriers (guarantee + free call + video), and UJ-1 beat 3 uses it, but FR-1 is satisfied by *any one* cue — the site could ship with no video and still pass FR-1 while quietly dropping a parent constraint.

## Findings by dimension

### 1. Decision-ready — strong

- Decisions section names each call, its date, and the alternative it beat (admin over static content, FR-4 over showing nothing, no blog, admin-never-layout deferred with a named trigger). The memlog confirms every one was the user's call or confirmed by them — nothing invented to fill a section.
- The thesis is stated once in the Vision ("the bet of this product") and every section serves it; the leanness note (admin is the heaviest piece, user chose it, OQ-7 is the named fallback) is a real trade-off honestly kept.
- **Low — Integrations and operations.** "Conversion-path availability … whose threshold the architecture sets" is correct altitude, but paired with the memlog's "SLOs — thresholds handed to architecture," the sentence reads as deferral without a named owner. Fix: one clause naming the booking/contact epic as where the availability threshold lands (it already owns OQ-1/OQ-3).

### 2. Earned — strong

- No persona theater: Mariam, Lina and Omar carry specific entry states, beats and failure paths; no padding sentences found. The glossary defines only nouns the features actually use.
- 2,806 words against the ~3,000 initiative ceiling — nothing to cut for length.
- **Low — Data and compliance / OQ-2.** The phone number is listed as collected unconditionally ("confirm you want to ask for it"), but its stated purpose ("used only to reply to the sender") contradicts its brainstorm origin as the spam-guard field. If OQ-2 settles on captcha or a confirmation step, the field should vanish. Fix: tie the phone field explicitly to OQ-2 ("collected only if the guard needs it") so the data list resolves with the decision.

### 3. Testable — adequate

- FR-1 through FR-5, FR-7 through FR-12 each state one checkable result; RULE-4 carries a real threshold (FMP under 2s, mid-range phone); RULE-6 gives the guard a checkable false-positive bound ("no more than the guard's single check"). SM-1 to SM-3 all name current-or-target values, the FRs they validate, and counter-metrics — above hobby stakes, satisfied.
- **High — RULE-5 vs FR-5.** RULE-5 covers only "a submitted contact message." A booking that reaches the scheduler but never reaches Hamza — the case the third-party-vs-custom-calendar fork (OQ-1) makes live — has no delivery guarantee, though the initiative's Boundary names booking notifications at the inbox. This is the document's own UJ-3 logic ("delivery, not just the confirmation screen, is the promise") left unapplied to the higher-stakes path. Fix: rewrite RULE-5 to cover both the form message and the booking notification.
- **Medium — UJ-1 resolution.** "Instant confirmation" after booking appears in the journey but in no FR (FR-8 is form-only). An epic building FR-5 from this PRD could ship booking without any on-screen confirmation. Fix: add the confirmation to FR-5 in one clause.
- **Medium — FR-1 vs initiative C1.** The intro video is one optional cue among five, while C1 makes video presence a named trust carrier and UJ-1 beat 3 leans on it. Fix: require the video in FR-1, or add a decision line stating the video is demoted to optional and why.
- **Medium — FR-6.** "Keeps the calendar free of floods of fake bookings" is a vague adjective where the initiative's own Done-when says the guard must *demonstrably* block a flooding attempt. Fix: name the observable at this altitude's grain — e.g., a scripted burst of fake bookings is blocked while a genuine booking completes in the same window — or explicitly hand the measurement to the booking epic in RULE-6's pattern.
- **Medium — RULE-7.** "A small fixed monthly amount" is untestable, and its own assumption tag says a number should be named — but no open question owns it, so the tagged guess has no resolution path (unlike SM targets, which OQ-6 carries). Fix: add the cost ceiling to OQ-6 or a new Hamza-owned OQ.
- **Low — SM-2.** "Share of discovery calls that become paying clients" has no attribution to site-sourced calls; Hamza will also get calls from elsewhere. At solo-operator stakes he can ask on the call, so this stays a nitpick: one clause ("calls that come from the site") would make it countable.

### 4. Honest scope — strong

- Six non-goals, each with the reason where it matters; the founding-client outreach and real-review exclusions correctly lift the initiative's boundaries. Out-of-scope MVP items carry reasons, and multi-language is tagged as an assumption rather than a silent cut.
- The fast-path guesses are all tagged (`[ASSUMPTION: …]` on RULE-4, RULE-7, SM targets, phone, geography, language) — this is what honest drafting on a fast path looks like.
- OQ-1 to OQ-7 each carry an owner, a when-needed, and what they block; OQ-5's "blocks nothing in the build / needed before launch" distinction is exactly right at these stakes.
- **Low — OQ-4.** Owner is "the opening epic," but no section names an epic called that (features are "First-screen trust," "Booking a discovery call," …). Fix: "the first-screen/build epic" or whatever name will last into the epic tree.

### 5. Usable downstream — strong

- All ids resolve and are bidirectional: every FR appears in at least one journey's realization list; every journey beat that needs the product traces (the two exceptions above); R1–R7 all land in FRs; C1–C3 land in RULE-1, RULE-3, RULE-2 respectively. TOC claims match section contents (7 features / 12 FRs; RULE-1–3 / RULE-4–7 split is stated in both places).
- Glossary terms are used exactly as defined; "scale figure," "translation line," "presence cue," "spam guard," "honest launch message" each do real work in the FRs and rules.
- The Decisions and OQ sections give the epic PRDs clean handoff points: they can cite FR-N/RULE-N/OQ-N rather than restating.
- **Low — memlog only.** The memlog records "some real projects may be un-publishable code … to be confirmed in Features" as an open assumption; the PRD resolved it into FR-4 and a Decision line but never marked the assumption settled in its own memlog. Fix (in the memlog, not the PRD): log the settlement so the assumption list stays trustworthy.

## Template `done when` check

- Every FR checkable at initiative altitude and tracing to a journey or need — met, with the FR-5-confirmation and RULE-5-booking exceptions noted.
- Every journey beat that needs the product has an FR — met except UJ-1's instant confirmation after booking.
- Non-goals and out-of-scope explicit — met.
- Counter-metrics on all success metrics — met.
- Shape fits the product — met (journey-led, correctly, per the memlog's user-narration decision).
- User remembers making every decision — met per memlog (names, admin area, no blog, rules, non-goals all confirmed by the user).
- Nothing invented to fill a section — met; the settled-out list is clean and the dropped sections are genuinely empty at these stakes.

## Verdict rating

Publishable after one small revision pass: the high-severity rule gap and the two medium confirmation/video gaps are each one-line fixes; no structural rework needed.
