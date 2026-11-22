# Technology-Currency Review — Architecture Spine (Software Engineer Portfolio)

- **Reviewer role:** technology-currency reviewer (subagent)
- **Date:** 2026-11-22
- **Document reviewed:** `architecture-portfolio.md` (status: draft, created 2026-11-22)
- **Method:** every committed Stack row and named technology was checked against current, primary or primary-adjacent sources via web search (as of 22 Nov 2026). Deferred-vendor free tiers were spot-checked because the spine names them as candidate options.

## Verdict

**PASS with corrections required.** The spine's version pins are, unusually, almost entirely reality-checked and correct — Django 6.1.3, DRF 3.18.3, React 19.3.x, Node 26 LTS "Lithium" are all exactly current. Two pins are stale (Next.js 16.3.x, PostgreSQL 17), one of which (Next.js) is now outside the vendor's security-patched release line and must be corrected before scaffold. One platform-fact risk (Vercel Hobby's non-commercial ToS clause) is unaddressed. No decision rests on a hallucinated or defunct technology.

## Findings

### F1 — Next.js "16.3.x" is stale and outside the security-patched line (HIGH — fix before scaffold)

The Stack pins **Next.js 16.3.x**. Current reality:

- Latest stable: **16.4.6** (published 9 Nov 2026; npm `latest` confirmed 21 Nov 2026).
- Next.js 16.4 shipped 6 Oct 2026; the **Active LTS line is 16.4.x**.
- The **14 Oct 2026 out-of-band security release** (two **critical unauthenticated RCEs** — one in `use cache`/typed arrays, one SVG-via-sharp — plus a high-severity Server Actions DoS) patched only **16.4.1 / 15.5.28** and later. No 16.0–16.3 version received a patched release.
- The **3 Nov 2026 scheduled security release** (seven advisories, one High) patched only **16.4.5 / 15.5.29**. All November affected ranges end at 16.4.4.

A project scaffolding on 16.3.x today would deploy a version with **no security patch path** for two 2026 critical RCEs. The spine must pin **16.4.x (≥ 16.4.6)**, or state "latest 16.4.x at scaffold time." Note this also strengthens AD-10: 16.4's Cache Components / revalidation behavior is the actively patched surface.

### F2 — PostgreSQL 17 is superseded; 18 is the current default (MEDIUM)

The Stack pins **PostgreSQL 17**. Current reality:

- **PostgreSQL 19 released 19 Nov 2026** (three days before this spine was written). Current: 19.0; 18.7; 17.12 (all current minor releases 12 Nov 2026).
- PostgreSQL 18 (Sep 2025) is the sensible default for a new database and is offered by managed providers today; PG 17 remains supported until Nov 2029, so the pin is *safe but not current*.
- Minor nit: the parenthetical "Django 6.1 floor is 15" is correct (Django 6.1 supports PostgreSQL via its normal floor), but the row reads as if 17 were the latest; it should say "17 or later; current majors are 18 (default) and 19 (released 2026-11-19)". Note PG 19 is 3 days old — most managed providers will lag it; **18 is the right committed target**.

### F3 — Vercel Hobby: numbers verified, but the non-commercial ToS clause is unaddressed (MEDIUM — flag, likely a doc note, not a redesign)

Hobby facts confirmed against Vercel docs (last updated 12 Nov 2026) and pricing page:

- 100 GB Fast Data Transfer, 10 GB Fast Origin Transfer, 1M CDN requests, 1M function invocations, 4 CPU-hrs, 360 GB-hrs provisioned memory, 5,000 image transformations, 100 deployments/day, 300 s max function duration, 1 concurrent build. All consistent with what the spine implicitly leans on.
- **Web Analytics on Hobby: 50,000 events/month included** (cookieless, R13 compatible — the spine's claim holds). Over-limit behavior: 3-day grace, then collection pauses for 7 days (shorter than the general 30-day pause). Worth one line in the spine so launch-week traffic spikes don't silently blind analytics.

The gap: Vercel's Terms of Service (§4, updated Oct 2026) state Hobby is for **"personal or non-commercial use only"** — being paid to build/host, ads, and client work count as commercial. A portfolio whose explicit purpose is **soliciting freelance work (booking, contact, 30-day guarantee)** is at minimum a gray zone. This doesn't invalidate the choice (personal portfolio sites are commonly hosted on Hobby), but the spine should record it as an accepted risk or name the Pro fallback ($20/mo) — otherwise the RULE-7 cost ceiling quietly depends on a ToS interpretation.

### F4 — Python 3.13 is supported but not current (LOW)

Python 3.14 (Oct 2025) is the current release; 3.13 is in support and Django 6.1 officially supports **3.12, 3.13, 3.14** (3.15 compatibility added in 6.1.3). Pinning 3.13 is defensible for library maturity, but the spine should either pin 3.14 (current) or note the choice is deliberate (ecosystem maturity), not "the current Python."

### F5 — Confirmed-correct pins (no action)

| Spine pin | Verified current fact | Status |
| --- | --- | --- |
| Django 6.1.x (6.1.3) | 6.1.3 is latest (3 Nov 2026 security release; six CVEs fixed — the pin includes them) | ✅ exact |
| DRF 3.18.x (3.18.3) | 3.18.3 latest (6 Oct 2026); supports Django 5.2/6.0/6.1 and Python 3.10–3.15; 3.18.2 fixed a HIGH parser DoS (GHSA-3547-9m27-7rxg) — pinning ≥3.18.2 matters and the pin does | ✅ exact |
| React 19.3.x | 19.3.1 latest (14 Oct 2026); 19.3 stable since 9 Sep 2026 | ✅ |
| Node 26 LTS (Lithium) | Active LTS since 28 Oct 2026; latest 26.13.1 (11 Nov 2026); EOL Apr 2029 | ✅ codename and status exact |
| Vercel Web Analytics (cookieless) | Cookieless, 50K events/mo free on Hobby | ✅ (see F3 pause behavior) |

### F6 — Deferred-vendor free tiers: currency facts to carry into the owning epics (ADVISORY — not committed, so not verdict-changing)

The spine defers these vendors but names candidates; current facts the owning epics will need:

- **Cloudinary free tier:** now a shared **25-credits/month** pool (1 credit = 1 GB storage OR 1 GB image bandwidth OR 1,000 transformations), rolling 30-day window, 10 MB max image file. Far tighter than commonly assumed — fine for a portfolio's screenshots, but "generous free tier" folklore is outdated.
- **Supabase Storage free tier:** **1 GB storage, 50 MB max upload, 5 GB egress** (+5 GB cached), and — the trap — **free projects pause after 1 week of inactivity**. If Supabase Storage is chosen for screenshots inside a Supabase project that isn't otherwise used, the pause rule applies to the whole project.
- **Transactional email (free/cheap):** Resend free = **3,000/mo with a hard 100/day cap** (UTC day), 3 domains, no overage on free. Adequate for contact-form volume (tens/day) with large headroom. Alternatives in the same bracket (Brevo, Postmark's 100/mo test tier) shift month to month — the epic should re-check at decision time.
- **Scheduler:** **Cal.com free = $0, 1 user, unlimited event types/calendars/bookings**, embed widget included — fits the spine's "third-party scheduler, one user" decision exactly. Calendly's free tier is narrower (1 event type) and weaker fit. Cal.com's free plan is officially scoped to *personal, non-production* use — same caveat shape as F3, smaller stakes.
- **Captcha:** **Cloudflare Turnstile free = unlimited challenges, no credit card, up to 20 widgets**, works without other Cloudflare services — the obvious default for the shared captcha port (AD-9). hCaptcha Basic is free to ~10K verifications/mo but shows challenges. reCAPTCHA's free tier is now 10K assessments/mo. None of this blocks the spine; it just means Turnstile is the current best-fit candidate when the contact-form epic decides.

## Summary table

| # | Severity | Item |
| --- | --- | --- |
| F1 | HIGH | Next.js 16.3.x → pin 16.4.x (≥16.4.6); 16.3 has no patch for 2026 critical RCEs |
| F2 | MEDIUM | PostgreSQL 17 → target 18 (current default); 19 released 2026-11-19 |
| F3 | MEDIUM | Vercel Hobby non-commercial ToS clause unaddressed; note analytics pause behavior |
| F4 | LOW | Python 3.13 supported but 3.14 is current; make the choice deliberate |
| F5 | — | Django 6.1.3, DRF 3.18.3, React 19.3.x, Node 26 LTS all verified exactly current |
| F6 | ADVISORY | Deferred vendors' free tiers carry current facts (Cloudinary 25 credits, Supabase 1-week pause, Resend 100/day, Cal.com free fits, Turnstile is the default captcha) |

## Sources checked (primary where possible)

- djangoproject.com download page + 6.1.3 release notes + weblog security announcement (3 Nov 2026)
- PyPI / GitHub releases for djangorestframework (3.18.3, Oct 2026)
- react.dev versions + GitHub releases (19.3.1)
- npm registry for `next` (16.4.6); nextjs.org blog (Oct/Nov 2026 security releases); Next.js support policy
- nodejs.org release index + nodejs/Release schedule (26.13.x 'Lithium' Active LTS)
- postgresql.org (19 released 19 Nov 2026; 18.7 / 17.12 current minors)
- vercel.com/docs/plans/hobby, vercel.com/docs/analytics/limits-and-pricing, vercel.com/pricing (all Nov 2026)
- cloudinary.com/pricing + billing docs; supabase.com/pricing; resend.com/pricing + quotas docs; cal.com/pricing; developers.cloudflare.com/turnstile/plans
