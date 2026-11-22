# Fresh-Reader Review — prd-portfolio.md

Reviewer role: freelance software engineer judging the document, plus the coding agents and designers who would build from it. Read cold, once, against one question: *can I build from this, and can I defend the result?*

## Verdict

This is a strong, unusually honest PRD — the trust-ladder structure, the truthful-stats rules, and the flagged assumptions make it defensible to build from, and the open questions are tracked rather than hidden. But it has one content hole it never confronts (what the site launches with when proof is empty), two promises the journeys make that no FR delivers (the intro video, booking notifications), and a few places where a builder must guess what the author meant. None of these sink the document; the first two need answers before the design and build epics start.

## Findings

### CRITICAL

**F-1. No answer for an empty proof ladder at launch.**
The document solves the "empty reviews" problem honestly (FR-9, the honest launch message, RULE-2). It never asks the same question about *projects*. The entire conversion bet — the vision, UJ-1 beats 2–3, UJ-2, SM-1 — rests on project pages with scale figures, screenshots, and repos. Nothing in the PRD states: how many projects version 1 launches with, whether scale figures exist for any of them yet, or what a visitor sees if the gallery is thin or empty. RULE-1 forbids fabricated numbers, so if Hamza has no truthful figures today, the proof scroll is the empty section the reviews section was designed not to be. A designer cannot draft the gallery and a builder cannot seed content without this. The PRD promises "every scroll answers the next doubt" — it owes the reader the answer for scroll two on day one.
*Fix:* Add a requirement or explicit assumption: minimum launch content (e.g. N projects, each with at least one truthful scale figure + translation line, screenshots, and a repo-or-confidentiality note), or give projects their own honest-empty fallback mirroring FR-9.

### HIGH

**F-2. The intro video is promised by the vision and journeys but required by no FR.**
The vision says "his voice on video"; UJ-1 beat 3 counts "an intro video with Hamza's voice" as part of the depth scroll; the glossary lists it as a presence cue. Yet FR-1 requires only "at least one presence cue" — and since the photo and the claim are separately mandatory, the cue is *everything else, all optional*. A designer building to FR-1 can legally ship without the video, and a reviewer judging against the vision can then call the work incomplete. The document contradicts itself: the video is central to the bet but structurally optional.
*Fix:* Either add an FR for the intro video (player, hosting, mobile behavior) or state explicitly in FR-1/the vision that the video is a post-launch presence cue, so builders and judges agree.

**F-3. A confirmed booking that Hamza never learns about is a silently lost conversion — RULE-5 protects the form, not the calendar.**
RULE-5 is the document's best rule: a contact message is delivered or the failure is surfaced to Hamza. But the calendar path — the *primary* conversion event, the one the product exists to cause — has no equivalent. "Booking notifications" appear once, in passing, in the Integrations section's description of the email service; no FR, no rule, no failure path covers a booking that lands in the scheduler but never reaches Hamza. UJ-1's failure path guards against fake bookings, not missed real ones. As written, the document treats the stronger conversion promise more loosely than the weaker one.
*Fix:* Extend RULE-5 (or add a rule) to cover booking notifications: every confirmed booking reaches Hamza through a channel whose delivery is checked, and a missed notification is a failed booking.

### MEDIUM

**F-4. "Contact form's target info" (FR-12) is undefined, and the admin area has no access-control requirement.**
FR-12 says Hamza can manage "the contact form's target info" — a builder must guess whether that means the destination email address, the form's field list, auto-reply text, or all three. Relatedly, the glossary promises a "small *private* area," and Non-goals say "no user roles beyond Hamza," but no requirement says how the admin area is protected (login, auth method, what happens on failed access). "Private" is asserted; nothing requires it. The document defers layout to architecture but security of its own admin surface is not explicitly deferred anywhere.
*Fix:* Define FR-12's "target info" concretely (at minimum: destination address + form fields), and add one line — even as a deferred-to-architecture pointer — stating the admin area requires authenticated access as Hamza only.

**F-5. The guarantee's scope is undefined, and the site copy must state it.**
FR-10 requires the site to state "free fixes for 30 days after delivery," and UJ-3's conversion hinges on Omar liking "the guarantee." But "fixes" is never bounded: bug fixes? any change? new feature requests? per-project or per-engagement? A designer writing the guarantee section — the promise the whole safety-net argument rests on — must invent terms, and an invented term that over-promises becomes a real liability in a paying engagement. The document that forbids fabricated trust elsewhere leaves its own central promise ambiguous.
*Fix:* Add one sentence defining the guarantee's scope (what counts as a fix, when the 30 days start, per what unit of work) — or flag it as an assumption/open question owned by Hamza.

### LOW

**F-6. "Presence cue" overlaps the two mandatory hero elements, muddying FR-1.**
The glossary defines a presence cue as "photo, positioning claim, intro video, fast-reply note, or video-meeting offer" — but FR-1 already mandates the photo and the claim separately. So "at least one presence cue" reduces to "one of: video, fast-reply note, video-meeting offer," which the reader must derive rather than read. A one-line clarification in FR-1 fixes it.
*Fix:* In FR-1, say "at least one further presence cue (intro video, fast-reply note, or video-meeting offer)" or list the eligible cues inline.

**F-7. SM-3's "booking requests" contradict the instant-booking journey.**
SM-3 requires replies to "form messages and booking requests" within 24 hours, but UJ-1's climax is an instant calendar booking with instant confirmation — there is no request for Hamza to reply to. Either the scheduler can produce requests awaiting approval (nowhere stated), or half of SM-3 measures nothing.
*Fix:* Reword SM-3 to cover form messages and (if the OQ-1 scheduler produces them) pending booking requests; otherwise drop "booking requests."

**F-8. Scale figures are required to be truthful (RULE-1) but nothing says how truth is evidenced.**
For client-confidential projects (the FR-4 edge case), the figures shown are exactly the ones hardest to verify and easiest to inflate. The document's honesty bet would be defended better by one line: each scale figure carries a source or basis (measured, client-reported, estimate) — possibly visible to the visitor, possibly admin-only. Optional, but cheap and on-brand.
*Fix:* Add an optional provenance note to RULE-1 or FR-11's admin fields.

## What is already good (so the builder knows what not to touch)

- The FR↔journey traceability is complete and checkable in both directions; a reviewer can verify coverage mechanically.
- Assumptions are flagged inline where they are made, not buried; OQ-6 (draft targets are mine, confirm them) is exactly the right posture.
- RULE-5 and the leanness note (FR-12 as the named first deferral, OQ-7) show the document already thinks about failure and overrun — rare and valuable.
- Non-goals are stated with reasons, which prevents scope arguments during the build.

## Summary judgment

Buildable after the open questions it already tracks (OQ-1–OQ-4) get answers — but before the design epic starts, someone must answer F-1 (launch content) and F-2 (is the video in or out), and before launch F-3 (booking notification guarantee) must become a rule, not prose.
