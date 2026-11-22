---
title: "Your Architecture, Explained — Software Engineer Portfolio"
type: explainer
companion-of: architecture-portfolio.md
audience: "Hamza — the site's builder and owner"
status: final
created: '2026-11-22'
---

# Your Architecture, Explained

This document walks you through every decision in the architecture spine, in plain language. The spine is the contract your seven epics will build against — terse by design. This is the human version: what each decision means, the jargon decoded, and the tradeoffs you weighed (or should know you're carrying).

One voice note before we start: **"I" is your coach from this session — the same person as the reviewers quoted below.** When a review "pushed back" on a choice, that was me (and independent review agents I dispatched) testing the decision before it became final.

**How to read it:** each decision gets four things — the call, the jargon, the tradeoff, and why it holds the project together. A couple of decisions merge two of those parts, but the shape is the same throughout.

**A key to the codes:** your planning documents number their requirements. `R-` numbers are the initiative's requirements, `FR-` its features, `RULE-` its quality rules, `UJ-` its user journeys, and `OQ-`/`Q-` its open questions. You never need to memorize them — they just point at numbered lines in your PRD and the spine.

**Your seven epics, in build order:** 1 foundation · 2 home trust · 3 project proof · 4 contact form + email · 5 booking · 6 admin · 7 launch (hosting, domain, cost). I'll refer to them by number or nickname from here on.

## The big picture first

Your site is **one Django application** (Python — the brain: database, email, bookings, security) behind **one Next.js application** (React — the face: what visitors and you see), talking to each other over one private channel. Three outside services carry the heavy lifting: a **scheduler** owns your booking calendar, an **email service** delivers messages, a **captcha service** stops bots. Your screenshots live in **object storage** — a file-hosting service — because cheap servers tend to wipe their disks.

That's it. Everything below is the fine print that keeps seven separately-built epics from drifting apart.

---

## AD-1 — One codebase, one database

**The call:** everything lives in one repository and one PostgreSQL database. When you edit a project in the admin, the public site reads that same data — there's no second copy anywhere.

**Jargon:** a *repository* is your project's folder under version control; a *database* is where structured data lives (tables, like smart spreadsheets).

**The tradeoff you weighed:** one application vs a split system. The split would have meant two things to host, two things to secure, and a "rebuild" step that could silently fail — serving stale or empty pages, exactly the silent failure your PRD forbids.

**Why it holds:** every epic — home page, projects, booking, contact — reads and writes the *same* tables. No epic can invent a rival version of "what a project is."

*Amendment you made later:* "one deployable" became two when you chose Next.js — but one codebase and one database still hold. Two things to host, still one brain.

## AD-2 — The write rule

**The call:** the database has exactly one writer: Django. Within Django, each module owns its own tables. The public site has exactly **one** door through which a stranger can add data — the contact form. Bookings never touch your database at all; they live in the scheduler's world.

**Jargon:** a *write path* is code that changes stored data. A *module* is a section of the Django app with its own job (messages, projects, content, accounts, integrations).

**The tradeoff you weighed:** should contact messages be stored, or just forwarded to your email? You chose **store + forward** — every message is saved *and* emailed. That buys you an audit trail: if email loses something, the database copy survives. The cost is holding visitors' personal data — which is why message retention is an open question you owe an answer to before launch.

**Why it holds:** your **attack surface** — the set of doors an attacker could walk through — stays tiny. A stranger can only ever create one kind of record. Everything else requires being logged in as you.

## AD-3 — The messages inbox

**The call:** the admin area shows a simple, read-only list of the messages people sent you — who, when, what, and whether the email delivery succeeded.

**Jargon:** the *inbox* here is just that list — a page in your admin area, not a second email account.

**The tradeoff you weighed:** this wasn't in the PRD. You chose to add it (it makes the stored messages actually usable), accepting a bit more build in the heaviest epic. It's deliberately minimal: a list, not an email client — no reply buttons, no folders.

**Why it holds:** this is a *flagged scope addition* — at the end of this run you should have Sofia update the ticket so the PRD and the build agree. A document that quietly disagrees with the code is how projects rot.

## AD-4 — The paradigm: modular monolith

**The call:** the architecture has a name — *modular monolith* — and the name carries rules: Django is divided into modules (public read, messaging, admin, accounts, integrations), and the arrows of dependency point one way only — inward, toward the core. The integrations module can be *used by* everyone but can *reach into* no one.

**Jargon:** a *monolith* is one application — as opposed to *microservices*, many small apps talking over the network. Microservices are the wrong tool at your scale, full stop. *Modular* means the one app is internally disciplined.

**The tradeoff you weighed:** none, really — this one was confirmed rather than chosen. The substance was already fixed by your earlier answers; naming it means every future builder reads "modular monolith" and knows the discipline for free.

**Why it holds:** "arrows point one way — inward" is checkable. An epic that violates it has written code you can point at.

## AD-5 — The stack: Next.js + Django

**The call:** Next.js 16 (React, server-rendered) is the face; Django 6.1 + DRF is the brain; PostgreSQL 18 is the memory.

**Jargon:** *Next.js* is a React framework that renders pages on a server — better speed and better Google visibility than a plain React app. *DRF* (Django REST Framework) turns Django into a JSON API — a waiter that takes requests from the frontend and returns data. *PostgreSQL* is the classic open-source relational database.

**The tradeoff you weighed — twice.** You first chose Java/Spring Boot, then switched to Django + React. Then came the real fork: React *everywhere* vs React *for the admin only*. I recommended admin-only (simpler, faster, safer for the 2-second rule — your PRD's demand that the first screen be usable within two seconds); you chose React everywhere, with Next.js to keep the rendering quality. You accepted: two frameworks to learn, two deployables to host, a heavier build. It's your learning project; the call is legitimately yours, and Next.js genuinely softens the speed and SEO costs that a plain React app would have imposed.

**One bookkeeping note:** the ticket had originally left the build-stack choice to the foundation epic's first story. Deciding it here moved that call into the spine — so the foundation epic now *installs* this stack rather than *choosing* one. That's one of the ticket updates Sofia should record.

**Why it holds:** every epic builds on the same framework, the same database, the same deployment shape — no epic can introduce a second stack "because it was easier that day."

**One consequence to know:** free Django hosting usually *sleeps* your server when idle, and waking it takes 30–60 seconds — which would break the 2-second rule exactly when a first-time visitor arrives. So the spine writes a hard constraint: **the Django server must stay warm** (a small always-on host, roughly $5/month), or you explicitly accept slow cold starts. Epic 7 decides the actual provider with this constraint in hand.

## AD-6 — One public origin, and how you log in

**The call:** everything the browser talks to lives under one web address. Your admin login is a Django **session cookie** — a credential the browser holds but JavaScript cannot read.

**Jargon:** an *origin* is a web address (scheme + domain + port). *httpOnly* means JavaScript can't touch the cookie — so even a successful XSS attack (malicious script injected into a page) can't steal your login. *CSRF* ("cross-site request forgery") is a trick where another site submits requests *as you*; Django's built-in defense handles it. A *session cookie* is a temporary login token the server issues when you sign in.

**The tradeoff you weighed:** session cookie vs JWT tokens (signed strings your React app would store and attach to requests). Tokens are the habit in single-page-app world; cookies are safer (unstealable) and less code. For a single-admin site, the cookie wins outright.

**Why it holds:** CORS (the browser's rule about which websites may call which) is the classic source of launch-day login failures. Routing all browser traffic through your own `/api/*` path removes CORS from the equation entirely. The foundation epic owns this plumbing once; every other epic just uses it.

## AD-7 — When delivery fails, you find out

**The call:** every email send records a status: `pending`, `sent`, or `failed`. A failed send shows up **marked** in your inbox *and* triggers a best-effort alert through an independent channel — deliberately *not* your main email vendor, so the alert survives the very vendor that failed.

**Jargon:** an *enum* is a field that can only hold one of a fixed list of values. A *second channel* is how you hear about a failure through a route other than the one that failed.

**The tradeoff you weighed:** you first chose inbox-only; the reviewer pushed back — a red row only helps if you open the admin, and RULE-5 says a lost submission must never be silent. You upgraded to the secondary alert. Cost: one more small integration. Benefit: a two-week silent email outage becomes loud within a day.

**Why it holds:** the booking notification uses the *same* email port and the *same* status field. One mechanism, one owner (`messaging`), no epic invents its own.

## AD-8 — Screenshots live in object storage

**The call:** project screenshots upload to a file-hosting service (object storage); Django stores only the URLs. The upload goes *through* Django — your browser never holds storage credentials. Uploads are capped at 10 MB.

**Jargon:** *object storage* is a service that holds files and serves them over URLs (Cloudinary and Supabase Storage are the friendly free-tier options). *Ephemeral disk* is the trap: many cheap hosts wipe the server's files on every redeploy. A portfolio that silently loses its own screenshots after each deploy would be destroying its proof — the thing the whole site exists to show.

**The tradeoff you weighed:** storing screenshots *inside* the database (no extra vendor, but bloat and custom code) vs object storage. Storage won for durability and simplicity.

**Why it holds:** the sequencing subtlety the reviewer caught is owned: the project-proof epic needs real screenshot URLs *before* the admin epic builds the upload UI. So the foundation epic sets up the storage port early, you name the vendor before the proof epic's content seed, and the admin epic fills in the upload screen. Nobody gets stuck waiting on anyone else.

## AD-9 — One adapter per vendor; the captcha verifies on the server

**The call:** one email service, one captcha vendor, one scheduler — each behind exactly one integration *port* (a single internal interface; the *adapter* behind it does the vendor's actual calls). And the captcha check runs **server-side**, in Django.

**Jargon:** a *port* is a plug socket in your code: the rest of the app says "send this email," never "send via Resend." Swap vendors next year and only the adapter changes.

**The tradeoff you weighed:** this decision quietly reshaped your booking flow. The ticket said "third-party scheduler" *and* "captcha guards the booking" — but if the booking widget lives on the scheduler's servers, your captcha has nothing to guard. The resolution: your site renders the open slots (from the scheduler's API), the visitor books through *your* form, Django verifies the captcha, then hands the booking to the scheduler with a short-lived signed request. A bit more build in the booking epic; in exchange, the guard actually guards, and the scheduler still owns every slot.

**Why it holds:** the ticket's own rule said the booking epic must reuse the contact epic's email adapter — this is that rule, made enforceable. Without it, two epics build two email integrations and you pay for both.

## AD-10 — Edits appear on the live site, fast

**The call:** public pages are server-rendered from the API, cached for speed. The moment you save an edit in the admin, Django tells Next.js "refresh these pages" (a *revalidation* call) — with a 60-second auto-refresh as the safety net.

**Jargon:** *server-rendered* means the HTML is assembled on the server before it reaches the browser — fast first paint (the moment something appears on screen) and good for Google. *On-demand revalidation* means the cache is rebuilt for exactly the affected pages, not the whole site.

**The tradeoff you weighed:** instant content vs speed. Pure speed would mean serving possibly-stale proof; pure freshness would mean rebuilding everything on every edit. The contract buys both: fast pages, fresh within seconds of a save.

**Why it holds:** the plumbing — the refresh route, the shared secret, the map of "which page shows which content" — is built *once, by the foundation epic*. The admin epic only pulls the trigger. Before this rule, two epics could each assume the other built the trigger, and your first admin edit would fire into a route that doesn't exist.

## AD-11 — A slow API must never stall the page

**The call:** every server-side fetch from Next.js to Django goes through one shared fetch layer with a 3-second timeout. If Django is slow or down, pages serve their last-known-good content instead of hanging. The video, the scheduler, analytics — no vendor call may ever block the first paint.

**Jargon:** *SSR* (server-side rendering) is what Next.js does — and it means every visitor's page load depends on a round trip to Django. A timeout with a graceful fallback is what turns that dependency from a liability into a non-event.

**The tradeoff you weighed:** strictness vs staleness. During an outage, visitors see slightly old content rather than an error page. For a trust-first site, stale beats broken.

**Why it holds:** the launch epic owns "keep Django warm and fast" — but it can't retrofit a fetch layer that pages 2 and 3 already built their own way. This rule makes the fetch layer foundation-owned from day one.

## AD-12 — The boring stuff that saves the site

**The call:** two environments (your dev copy, and production — the one visitors see) — no staging, deploy = merge to main. Database backups: the provider's automatic daily backups, with one restore *actually tested* before launch. Monitoring: a free uptime monitor on the API and the **conversion paths** (the booking and contact routes a paying client walks), the failed-delivery rows, and Vercel's analytics. The conversion path carries a **99% monthly availability** target.

**Jargon:** an *environment* is a running copy of the site. A *restore test* means proving the backups work — an untested backup is a hope, not a backup.

**The tradeoff you weighed:** you chose to skip staging (right call for a solo build — staging is overhead you don't need) — but the reviewer caught that environments, backups, and monitoring had gone *silent* in the spine, and silence is how they never get built. Now they're named, owned, and scheduled.

**Why it holds:** the PRD asked the architecture to set an availability threshold for the booking and contact paths — downtime there is lost clients. 99% monthly is the honest, modest number: about 7 hours of allowed downtime a month.

---

## The conventions: small rules that prevent big arguments

These bind without earning their own AD. The ones worth knowing by name:

- **Every number is stored with its meaning.** A scale figure — a truthful number about a project, like "users handled" — is stored as three inseparable parts: the figure, its unit, and its plain-language translation line. RULE-1 ("a number without its meaning does not ship") is enforced by the data model itself, not by anyone's vigilance.
- **Slugs are forever** — a project's web address never changes after creation; renaming a project never breaks a link someone already shared.
- **A project proves itself or says why not** — every project carries either a public repo link or a written reason it can't have one (FR-4), enforced by validation.
- **No hardcoded copy on the home page** — every word a visitor reads comes from the database, so the admin can edit it. Section keys live in one registry owned by the home-trust epic; later epics reference keys, never invent rows.
- **Your contact email lives in the database, not in config** — so you can change it from the admin (FR-12). Config holds secrets; the database holds business decisions.
- **One error shape, one list of error codes, one list of event names, one env-var template** — all owned by the foundation epic, so four epics don't invent four vocabularies.
- **Mobile-first** — the site is built for the phone first (UJ-3, the journey that happens on a phone during a slow afternoon), with desktop as the equivalent experience.

## The three changes you made along the way (and what they cost)

Honesty section, because an explainer that only celebrates would be selling:

1. **Java/Spring Boot → Django + React.** Your first choice was a good learning bet; the switch was about admin ease and frontend modernity. Cost: two frameworks to learn instead of one deep one. Benefit: Django's batteries and a frontend that matches the job market.
2. **React everywhere (against my lean).** Cost: as flagged in AD-1 and AD-5 — two deployables, a cold-start constraint — plus two frameworks to learn. Benefit: one React codebase to learn deeply, server-rendered quality via Next.js.
3. **Store + forward + inbox.** Cost: personal data in your database (retention question owed), a small inbox feature beyond the PRD. Benefit: an audit trail that makes silent message loss impossible — which is the exact failure your PRD's UJ-3 names as the trust-killer.

None of these was wrong. All three were *chosen with the tradeoff on the table*, which is the only standard that matters for decisions you'll live with.

## What was deliberately *not* decided

A spine that decides everything decides nothing well. These are left open, each with an owner and a deadline:

| Open item | Owner | When it's owed |
| --- | --- | --- |
| Hosting provider + region | epic 7 | at the hosting decision |
| The cost ceiling number (RULE-7) | you | at the hosting decision — the warm-server figure in AD-5 (~$5/mo) is the first input to it |
| Email vendor | contact epic | its build |
| Captcha vendor | contact epic | its build |
| Which scheduler | booking epic | its build |
| Media-storage vendor | you name it; the admin epic builds the upload screen | before epic 3's content seed |
| Message retention + privacy statement + EU question (OQ-5) | you | before launch |
| Deep SEO work | version 1.1 | only if search traffic matters |

## What happens next

The spine is final. The recommended next step is **Sofia** (`bmad-agent-po`), in three moves:

1. Adopt the spine as a companion of the initiative ticket.
2. Record the ticket updates the reviewers surfaced: the messages inbox scope addition, the stack decision moving out of the foundation epic's first story, and the booking flow's new shape (site-rendered slots + captcha + signed handoff instead of a pure scheduler embed).
3. Start epic 1 (foundation) with `bmad-build` — it now has an architecture to build against instead of assumptions to argue with.
