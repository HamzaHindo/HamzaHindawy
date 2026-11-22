# Rubric Review — architecture-portfolio.md

- **Reviewed:** `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md` (199 lines)
- **Against:** `_bmad-output/initiative-portfolio/initiative-portfolio.md` (initiative, 7 epics)
- **Date:** 2026-11-22
- **Reviewer:** rubric walker (subagent)

## Verdict: REVISE

The spine is genuinely strong on the hard part — architecture-level divergence between epics. AD-10 (content freshness across two deployables), AD-9 (one adapter per shared vendor), and AD-2 (the write rule) are exactly the kind of decisions that stop seven epics from inventing seven answers. But it fails on two rubric items: the capability map misnumbers the ticket's requirements and silently drops R9, and the operational envelope the doc claims as its own is mostly silent (environments, backups, monitoring).

---

## Item 1 — Fixes the real divergence points for the level below; misses none

**FAIL** (one real miss, one small seed)

**What it fixes well:**
- **AD-10** (content freshness across the two deployables) is the subtlest and most valuable divergence point in this initiative — an admin edit in epic 6 must reach Next.js pages rendered by epics 2/3. Deciding SSR + on-demand revalidation webhook + 60 s fallback here prevents each consumer epic from inventing its own cache strategy. Excellent.
- **AD-9** (one adapter per shared vendor) directly prevents the booking epic (epic 5) duplicating the contact epic's (epic 4) email adapter or adding a second captcha.
- **AD-2** (one writer; contact = store + forward; booking never touches the DB) kills the most likely divergence: booking data leaking into local tables.
- **AD-6** (Django session cookie, no JWT) prevents per-epic auth inventions between foundation and admin.

**The miss — cross-deployable URL topology is undecided and two ADs presuppose it:**
The spine fixes two deployables (Vercel Next.js + a Python-hosted Django API) but never decides whether they share an origin (e.g. same domain, API under a path or subdomain behind a proxy) or live on separate origins. This is not a detail:
- **AD-6** commits to `SameSite=None` + credentials cross-origin + CSRF trusted origins — a choice that is only *necessary* if the deployables are cross-origin. If a same-origin routing decision were made, `SameSite=Lax` (the safe default) would work and the whole cross-origin CSRF/CORS surface shrinks. The AD hardcodes the riskier configuration without the topology decision that justifies it.
- AD-10's revalidation webhook and the error-envelope convention both need to know the origin relationship too.
Each epic touching auth (foundation, admin) or revalidation (foundation, admin) will otherwise resolve this independently. This belongs at this altitude.

**Small seed — launch screenshots predate the upload path:** AD-8 binds project-proof (epic 3) and admin (epic 6). Epic 3 must render the launch project's screenshots, but the upload UI lives in epic 6. How the launch project's media gets into object storage (manual upload? fixtures?) is unspecified, and epic 3 is exactly where a forbidden fallback (local disk, base64-in-DB) would sprout. One sentence would close it.

## Item 2 — Every AD's Rule is enforceable and prevents its stated divergence

**PASS** (with notes, none blocking)

- AD-1…AD-10 all state rules that are mechanically checkable at review time: import-lint for AD-4's dependency direction, a single `save()`/write path for AD-2, cookie flags for AD-6, delivery-status field for AD-7, URL-only media for AD-8.
- AD-1 is amended by AD-5 ("one deployable no longer holds") — an AD amended by a later AD is a smell, but the amendment is explicit and preserves the binding parts (one codebase, one DB). Acceptable.
- AD-2's "exactly one writer" survives scrutiny: Django's session table writes flow through the admin module's auth endpoints, so the invariant holds.
- AD-7 correctly prevents both stated divergences in both directions (swallowed failures *and* an unprompted second alerting service).
- Cosmetic: AD-3's "**PrEvents**:" typo (should be "Prevents").
- Note: AD-3 adds scope beyond FR-11/FR-12 (a read-only inbox). It is honestly flagged as a deliberate addition routed to Sofia — correct altitude behavior, not a violation.

## Item 3 — Nothing under Deferred could let two units diverge

**PASS** (one watch item)

- Email vendor + captcha → contact-form epic; scheduler vendor + embed shape → booking epic: safe. The port and the one-vendor rule are fixed by AD-9, and build order (contact epic 4 before booking epic 5) means booking *consumes* the contact epic's adapter rather than racing it. Matches the ticket's slicing decision ("booking epic waits on the contact epic's choice").
- Python host + region → epic 7: legitimate, honors the owner's 2026-11-22 decision, and the warm-API constraint and RULE-7 pressure are noted as already fixed. (See Item 6 for the constraint's missing owner.)
- Media-storage vendor → admin epic: the invariant (AD-8) is fixed so the vendor choice can't fork the data model. Safe *as a vendor deferral* — but see the epic-3/epic-6 screenshot seed under Item 1, which is adjacent to this row and unaddressed.
- Retention → before launch (owner): correct, matches OQ-5; correctly identified as blocking nothing in the build.
- DRF patch pins → foundation: correct; seed pins the minor line.
- Deep SEO → v1.1; leanness fallback → owner: neither is an inter-unit decision.
No deferred row lets two units independently pick conflicting answers.

## Item 4 — Named tech is verified-current

**FAIL** (one stale pin; everything else verified against primary sources)

Verified current as of 2026-11-22:

| Claim | Verdict |
| --- | --- |
| Django 6.1.x (6.1.3) | ✅ 6.1.3 is the latest release (Nov 3, 2026); supported to Apr 2027 |
| "Django 6.1 floor is PostgreSQL 15" | ✅ Verified — Django 6.1 dropped PostgreSQL 14 support |
| DRF 3.18.x (3.18.3) | ✅ Latest (Oct 6, 2026); officially supports Django 6.1 |
| React 19.3.x | ✅ Latest is 19.3.1 (Oct 14, 2026) |
| Node.js 26 LTS (Lithium) | ✅ Active LTS since Oct 28, 2026 |
| Python 3.13 | ✅ Supported by Django 6.1 (which also supports 3.14 — 3.13 is a defensible choice, not an error) |
| PostgreSQL 17 | ✅ Supported (18 is current since Sep 2025; 17 on a managed provider is fine) |
| **Next.js 16.3.x** | ⚠️ **Stale.** 16.3 exists (Aug 3, 2026) but the current stable/LTS line is 16.4.x (16.4.6). 16.4.1 shipped **critical RCE security fixes**; the 16.3 branch's last patch is 16.3.8 (Sep 30, 2026). Pinning a two-minors-old line for a greenfield build, in a framework with this security cadence, is the one pin this spine gets wrong. |

Also: every version in the Stack table is asserted with no source or "verified on" note. All of them happened to check out — but the checklist asks that assertions carry evidence, and one of eight didn't survive the check. Add provenance (or a "verified <date>" note) to the table.

## Item 5 — Covers every requirement in the ticket (R1–R13, C1–C9, RULE-1–7)

**FAIL** (R9 dropped; requirement ids misaligned; RULE-3 and RULE-4 weak)

**R9 is missing.** The Capability → Architecture Map has no row for R9 (FR-14): the launch gallery carries at least one real project with a truthful scale figure, and grows through the admin area. This is the one requirement the ticket explicitly *splits across epics* ("R9 splits launch part (epic 3) from growth part (epic 6)") — the highest-risk requirement for cross-epic divergence — and the spine never maps it. It is arguably implied by the `projects` module row, but implication is not coverage at this altitude.

**Requirement ids in the map contradict the ticket it binds.** Frontmatter binds R1–R13, but from the fourth row the map's ids shift off the ticket's numbering:

| Map row says | Ticket actually says |
| --- | --- |
| "R4, FR-4 (evidence, repo link)" | FR-4 is ticket **R3**; ticket R4 is booking (FR-5/6) |
| "R5 (booking + spam guard)" | Booking is ticket **R4** |
| "R6, R5-contact (contact form)" | Contact form is ticket **R5** |
| "R7 (reviews, honest launch message)" | Reviews is ticket **R6** |
| "R10 (30-day guarantee)" | Guarantee is ticket **R7** |
| "R11, R12, FR-15 (admin area)" | Admin is ticket **R10** (FR-11), **R11** (FR-12), **R12** (FR-15) |

Downstream epics that trace requirements by id through this map will bind to the wrong ticket items. Rows 1, 2 and 9 (R1/R8, R2/R3, R13) happen to align, which makes the drift harder to spot, not safer.

**RULE/C gaps:**
- **RULE-3 / C2** (calls are free, 10–15 min, no commitment): never referenced anywhere. Mostly epic-copy, but the third-party scheduler decision interacts with it (the scheduler must offer free 15-min slots), and the spine cites every other RULE — its silence here reads as an oversight.
- **RULE-4 / C5** (first meaningful paint < 2 s on a mid-range phone): only "RULE-4 via hosting constraint" — a gesture, not a decision. The SSR choice and 60 s revalidate window are real levers, but no performance budget, no measurement method, and no verification owner is fixed. The ticket's constraint is testable; the spine leaves it untestable.
- **RULE-7 / C8** (cost ceiling): AD-1 cites RULE-7 for hosting cost, and the Deferred table claims "the RULE-7 ceiling [is] already fixed here." It is not — OQ-9 leaves the ceiling open with the owner. Overstating coverage of a constraint is worse than deferring it.
- **C9** (mobile-first, no native app): silent. Mostly epic-level UX, and "no native app" is implied by the web stack, so minor.
- **C1 / C4** (trust without fabricated reviews): covered indirectly via the RULE-2 reference in the R7/reviews row; content-level, acceptable.

**Covered well:** RULE-1 via the scale-figure triple ("a number without its translation line cannot be represented" — invariant by construction, the best pattern in the doc), RULE-5 via AD-7, RULE-6 via the single-captcha-check convention, C6/C7 fully.

## Item 6 — Every owned dimension decided, deferred, or an open question

**FAIL** (the biggest finding)

The Structural Seed's own caption claims "Deployment topology (the operational envelope this altitude owns…)" — and then leaves most of that envelope silent. Walking the named dimensions:

| Dimension | Status |
| --- | --- |
| Deployment topology | ✅ Decided: two deployables, Vercel UI + deferred Python host; diagrammed. Legitimately defers the host per the owner's decision. |
| Environments | ❌ **Silent.** No dev/staging/production strategy — not decided, not deferred, not an open question. For a system with a revalidation webhook between two deployables and a same-origin/cross-origin question pending (Item 1), environments are a real initiative-level decision. Epic 7 will have to invent one. |
| Backups | ❌ **Silent.** AD-1 makes PostgreSQL the single source of truth for *all* proof content (projects, screenshots-urls, section text, messages). A lost database is a lost portfolio. Backup policy for the managed Postgres is an initiative-owned decision appearing nowhere — not even in Deferred with an owner and a "needed before launch" date, which is exactly the format the table exists to provide. |
| Monitoring / operations | ❌ **Silent.** AD-7 covers email delivery status only. No error tracking, uptime check, or log story for the warm API. "Hamza knows the site is down" is an initiative-level concern for a one-person business whose site is its funnel. |
| Infra/provider strategy | ⚠️ Partial. Host deferral is legitimate; but the **"must stay warm"** constraint is load-bearing (it gates the host choice, cold-start UX on booking/contact, and free-tier feasibility) yet has no owner, no mechanism (keep-alive? minimum instance?), and no stated fallback if the deferred host can't guarantee it. A constraint everyone depends on and no one owns is a divergence point. |
| Secrets/config | ✅ Decided (env vars, never committed). |
| Domain/DNS | ❌ Silent. The ticket assigns domain to epic 7, and OQ-5's region half is deferred — but the spine's Deferred table never carries domain/DNS at all, and the region deferral is half-specified ("OQ-5's region half") with the personal-data rationale living only in the ticket. |

A whole-dimension silence is a finding by the rubric's own standard; here it is three dimensions (environments, backups, monitoring/operations) plus domain.

## Item 7 — Diagrams valid mermaid and convey structure; prose only where it must

**PASS** (two semantic nits)

- All three diagrams are syntactically valid mermaid: `graph TD` with a dotted negation edge and cylinder shape, `graph LR` with subgraphs, and an `erDiagram` — no syntax errors found.
- **erDiagram cardinality errors:**
  - `CONTACT_SETTINGS ||--|| MESSAGE` asserts *exactly one* message per settings row. The truth is one settings row to many messages: `||--o{`. As drawn, the diagram says the inbox (AD-3) can hold a single message.
  - `SECTION_TEXT ||--o{ SECTION : "one row per editable section"` — the gloss describes SECTION's rows, not a 1-many from SECTION_TEXT; the relationship as drawn is inverted or at best confusing.
- **Deployment graph nit:** `NX --> SC[Scheduler widget]` draws the Next.js app connecting to the scheduler. In reality the visitor's browser loads the third-party widget directly; the edge misplaces the integration and sits oddly against AD-2/AD-4's client-of-the-API framing.
- Prose discipline is good: diagrams carry structure; prose carries rules and only-what-must-be-prose (AD rules, conventions). No prose-where-a-diagram-belongs violations.

---

## Findings summary (ordered by severity)

1. **[Item 6] Operational envelope silent:** environments, backups, and monitoring/operations have no decision, deferral, or open question — despite the doc explicitly claiming deployment topology as "the operational envelope this altitude owns."
2. **[Item 5] Capability map misnumbers ticket requirements** from R4 onward (map "R4" = ticket R3, etc.) and **omits R9 entirely** — the requirement the ticket explicitly splits across epics 3 and 6.
3. **[Item 1] Cross-deployable URL topology undecided** (same-origin proxy vs separate API origin), while AD-6 hardcodes the `SameSite=None` cross-origin cookie configuration that this decision should justify.
4. **[Item 4] Next.js pinned 16.3.x — stale:** current line is 16.4.x (16.4.6), and 16.4.1 carried critical RCE fixes. All other pins verified current against primary sources.
5. **[Items 2/5/7 — minor] RULE-7 ceiling claimed fixed but open (OQ-9); RULE-3/C2 and RULE-4/C5 unaddressed;** epic-3 screenshots precede the epic-6 upload path with no stated bridge; erDiagram cardinalities (`CONTACT_SETTINGS ||--|| MESSAGE`) wrong; "PrEvents" typo in AD-3.

## What not to change

AD-10, AD-9, AD-2, and the scale-figure-triple convention are the spine's load-bearing walls — precise, enforceable, and correctly altitude-ed. The honest flagging of AD-3's scope addition to Sofia and the explicit AD-1 amendment are model behavior for how an initiative spine should handle drift.
