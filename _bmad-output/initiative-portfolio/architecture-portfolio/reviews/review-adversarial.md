# Adversarial Architecture Review — architecture-portfolio.md

- **Reviewer role:** adversarial architecture reviewer (red team)
- **Date:** 2026-11-22
- **Input:** `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md` plus the seven epic tickets under `_bmad-output/initiative-portfolio/tickets/`
- **Method:** for each pair of sibling epics, construct the most compliant reading of the spine each agent could hold, then show the two readings collide. Every finding below is one both-epics-obey-the-letter scenario, not a strawman.

## Verdict

**Not yet build-safe for independent agents.** The spine fixes the right invariants (single DB, one writer deployable, vendor ports, freshness contract), but six of its rules are stated one abstraction level too loose, and three pieces of plumbing nobody owns (CORS, the revalidation webhook, the captcha handoff to a third-party scheduler). Seven independent agents can each satisfy every AD as written and still produce a system that does not assemble. Findings F1, F2, F3, F5, F8 are blocking; the rest are strong recommendations. Each finding ends with a proposed closing rule, ready to paste as a new or amended AD / convention.

---

## F1 — AD-2 contradicts itself: who writes the contact message? (contact-form vs admin)

**Pair:** epic 4 (contact form) vs epic 6 (admin)

**Both-compliant readings that collide:**

- Epic 4's agent reads the **Structural Seed source tree**, which lists a `messaging/` module ("contact messages, delivery status, inbox endpoints"), and AD-4's module diagram. It builds `POST /api/messages/` inside the `messaging` module, which writes the `MESSAGE` table. Compliant with AD-1, AD-5, AD-9.
- Epic 6's agent reads **AD-2 literally**: "the database has exactly one writer: the admin module." It concludes all DB writes — including message intake — must physically live in the `admin` module, and builds the message-intake endpoint there (or expects the `messaging` tables to be written only from admin-module code). Also compliant.

The clash: the public contact form is an **unauthenticated public write**, which AD-2 as written forbids ("the public surface never persists anything") yet AD-3 and the contact epic require ("a contact message is stored in the DB"). One agent ships a public write path in `messaging/` (a "second writer" under a literal AD-2), the other ships intake behind admin auth (the public form cannot submit). Neither violates a word of the spine. Additionally the diagram line `PUB -.->|never writes| DB` conflicts with the seed tree putting messages in `messaging/`, not `admin/`.

**Proposed closing rule (amend AD-2):**
> AD-2 (amended): "One writer" means one writing *deployable* (Django) and one authorization surface. Within Django, each module owns writes to its own tables (`messaging` writes messages, `projects` writes projects, `content` writes content, `accounts` writes users). The public surface has exactly one write endpoint — the contact intake `POST /api/messages/` in the `messaging` module, captcha-guarded — and no other public endpoint may mutate state. Every other write goes through authenticated admin endpoints. The UI never writes the database.

## F2 — AD-7 is split-brained: booking notifications and the delivery-status shape (contact-form vs booking vs admin)

**Pair:** epic 4 vs epic 5 (notifications), epic 4 vs epic 6 (status field)

**Both-compliant readings that collide:**

- AD-7 says "Booking notifications ride the scheduler's own delivery, checkable in the scheduler dashboard." Epic 5's agent therefore builds **no email send at all** — the scheduler emails Hamza, done. Compliant with AD-7.
- Epic 5's ticket says "A completed booking reaches Hamza as a notification through the **email service epic 4 settled**." Epic 5's agent therefore calls the shared email port per AD-9 and records a delivery status. Also compliant.
- Delivery-status shape: AD-7 names only the values "(sent / failed)". Epic 4's agent stores a boolean `email_ok`; epic 6's agent renders an enum `{pending, sent, failed}` and builds the inbox against it. Both satisfy "records a delivery status."
- Second-channel semantics: AD-7 says "failed sends appear marked in the admin messages inbox" — but bookings never touch the DB (AD-2), so there is no message row to mark. Epic 5's agent must invent a second-channel story the spine doesn't define (log-only? a pseudo-message row? nothing?).

The clash: two different notification architectures for bookings, and two different delivery-status data shapes, each individually AD-conformant.

**Proposed closing rule (amend AD-7):**
> AD-7 (amended): Booking notifications are sent by the booking epic through the shared email port (AD-9); scheduler-native confirmation emails are supplementary and out of RULE-5's scope. Delivery status is a single field `delivery_status` on `messaging.Message`, enum `pending | sent | failed`, owned and written only by the `messaging` module (contact-form epic); the admin inbox renders it read-only. `pending` is the initial state on intake; no automatic retry exists in MVP; a failed booking notification surfaces through the same failed-send mechanism as contact messages (log + admin-visible entry), not the scheduler dashboard.

## F3 — The AD-10 revalidation webhook is plumbing nobody owns (foundation vs admin, consumed by 2/3/4)

**Pair:** epic 1 (foundation) vs epic 6 (admin)

**Both-compliant readings that collide:**

- AD-10 binds "foundation epic (plumbing), admin epic (trigger)". But epic 1's ticket scope is: scaffold, analytics, sign-in. Its boundaries name exactly one touch point — the analytics service. Epic 1's agent builds **no revalidation endpoint and no webhook client**, compliant with its ticket.
- Epic 6's agent reads AD-10's "a Django admin save triggers on-demand revalidation" and implements the trigger — assuming the receiving end (the Next.js revalidation route, the shared secret, the env vars) already exists from epic 1. Compliant with AD-10 and with its ticket (the ticket never mentions revalidation either).

The clash: at assembly time the trigger fires into a route that was never built — or worse, epic 6 builds *both* halves inside the admin epic and epic 2/3/4 know nothing about the page→path mapping. Worse still, "affected pages" is undefined: who maps "edited the hero section" → "revalidate `/` and `/`'s ISR deps"? Epic 2 owns the pages, epic 6 owns the save; the mapping is a third thing with no owner.

**Proposed closing rule (amend AD-10):**
> AD-10 (amended): The revalidation plumbing is one deliverable owned by the foundation epic: (a) a token-protected revalidation route in `web/`, (b) a `revalidation` port in the Django `integrations` module with a no-op dev adapter, (c) a single page-path map in `web/` keyed by content entity (`section:<key>`, `project:<slug>`). Admin epics only call the port after a successful save; the contact-form and home-trust/project-proof epics extend the path map when they add pages. The webhook secret is an env var on both deployables, documented by foundation.

## F4 — The captcha guard cannot be wired to a third-party scheduler embed, and nobody says what it guards (contact-form vs booking)

**Pair:** epic 4 vs epic 5

**Both-compliant readings that collide:**

- Epic 4's agent implements the captcha port as a **server-side verify inside Django**: the SPA gets a token, `POST /api/messages/` verifies it before intake. Compliant with AD-9, RULE-6, conventions ("captcha check is the single guard check").
- The spine's own topology diagram routes booking **directly from the browser to the scheduler** (`NX --> SC`, "booking lives here") and AD-2 says a booking never touches the DB. Epic 5's agent cannot verify a captcha server-side in Django for a flow that bypasses Django. Its compliant options: (a) client-side-only check in front of the embed (violates the spirit of "single guard check" enforcement, satisfies the letter), (b) proxy the booking handoff through Django — creating a **second public write path AD-2 never anticipated**, (c) declare the scheduler's built-in anti-spam as "the guard" — contradicting AD-9's "one captcha vendor … shared with booking".
- Sub-clash: who exposes the captcha **site key** to the browser? Epic 4 owns the vendor; the conventions say config via env vars — but env vars of which deployable, and surfaced how? Each epic picks its own mechanism.

The clash: two different guard placements, and possibly a rogue public write endpoint.

**Proposed closing rule (amend AD-9 + conventions):**
> AD-9 (amended): The captcha port exposes a Django-side token verification. The single guard check on the booking path is executed server-side in Django at the booking handoff: the SPA obtains a captcha token, exchanges it at a Django endpoint for a short-lived signed booking handoff, and only then loads/prefills the scheduler embed; the scheduler's native anti-spam is out of RULE-6 scope and is not a second guard. The captcha site key is served to the SPA via one public config endpoint owned by the foundation epic; the secret key is a Django env var only.

## F5 — Auth surface: login endpoint shape, CSRF trusted origins, and CORS-with-credentials have no owner (foundation vs admin vs launch)

**Pair:** epic 1 vs epic 6 (vs epic 7)

**Both-compliant readings that collide:**

- AD-6 binds foundation "(setup)" and admin. Epic 1's agent implements "a sign-in exists that only Hamza can pass" the cheapest Django way: the **built-in Django admin site's login form** (server-rendered, session cookie) — compliant with AD-6's letter ("Django's session framework").
- Epic 6's agent builds the React admin SPA, which needs a **JSON login endpoint** (`POST /api/auth/login`), a CSRF cookie handoff, and `credentials: 'include'` fetches. Compliant with AD-6 and AD-5.
- Neither epic owns **CORS with credentials**: nothing in the spine mentions CORS at all. Epic 1 may ship Django with no CORS layer (its ticket doesn't ask for one); epic 6's SPA then cannot authenticate cross-origin. Or both add CORS config with different origin lists.
- **CSRF trusted origins** must include the Vercel domain — unknowable until epic 7 picks hosting. Nobody owns that config.
- Convention gap: the API base-URL env var name (`NEXT_PUBLIC_API_BASE_URL` vs `API_URL` vs …) is invented by whichever epic fetches first (epic 2).

The clash: a working sign-in that the admin SPA cannot use; a CORS layer built twice or not at all; CSRF failures appearing only at launch.

**Proposed closing rule (amend AD-6 + new convention rows):**
> AD-6 (amended): The foundation epic owns the entire auth surface as JSON endpoints in the `accounts` module under `/api/auth/` (login, logout, session check), the CSRF cookie handoff, and the Django built-in admin site is disabled. CORS is part of foundation's scaffold: Django allows credentials from exactly one UI origin, configured via env vars `CORS_ALLOWED_ORIGINS` / `CSRF_TRUSTED_ORIGINS`, populated at launch (epic 7). The admin SPA consumes only these endpoints and never implements auth logic. The API base-URL env var names are fixed by foundation (`NEXT_PUBLIC_API_BASE_URL` in `web/`, `…` in `api/`).

## F6 — Cross-deployable runtime contract: SSR fetch timeout, fallback rendering, and who keeps Django warm (home-trust/project-proof vs launch)

**Pair:** epics 2–3 vs epic 7

**Both-compliant readings that collide:**

- Epic 2's agent builds SSR pages fetching `/api/sections/` with framework defaults (no explicit timeout, hard failure on error). Compliant with AD-5, AD-10.
- Epic 7's agent must hit FMP < 2 s on a mid-range phone (C5) against a **self-hosted Django that "must stay warm"** — the spine states the constraint but assigns no mechanism. A cold Django (or a slow free host) turns every SSR request into a multi-second stall; Vercel Hobby function limits compound it. Epic 7's ticket explicitly says it "does not rebuild" what epics 1–6 built — so it cannot retrofit the fetch layer.
- Nobody owns the degraded-mode rule: when Django is slow/down, does the page render stale ISR content, static fallback copy, or a 500? Epic 2 and epic 3 will each pick differently (e.g. epic 2 renders fallback hero copy; epic 3 renders an error state).

The clash: a fetch layer with no timeout/fallback policy meets a latency constraint it was never built for, and two pages degrade differently.

**Proposed closing rule (new AD-11):**
> AD-11 — SSR fetch budget and degradation: all server-side fetches from Next.js to Django go through one shared data-fetch layer built by the foundation epic, with a fixed per-request timeout (e.g. 3 s) and one documented degradation rule (serve last-good cached content; never block FMP on a vendor call such as the scheduler widget or analytics). Keeping Django warm (uptime target, keep-alive mechanism, monitoring) is a launch-epic deliverable; no other epic may add per-page workarounds.

## F7 — Section-text keys: the contact section is owned by two epics and the reviews message may not exist as data (home-trust vs contact-form vs admin)

**Pair:** epic 2 vs epic 4 (vs epic 6)

**Both-compliant readings that collide:**

- The convention says "one row per editable section" but never defines the key namespace or who creates rows. Epic 2's agent builds the scroll **including the section shell that the contact form closes** ("the form section closes the scroll that epic builds") and seeds a content row for it — say key `contact`. Compliant.
- Epic 4's agent adds its own section copy for the form — say key `contact_form` — as a new `SECTION_TEXT` row. Also compliant: nothing forbids creating rows, and epic 4 "owns" the contact requirement (R5).
- Result: two rows covering one visual section; the admin inbox of section editors (epic 6) shows conflicting copies, and whichever row epic 6's editor writes may not be the one the page renders.
- Second collision in the same pair: the "honest launch message" (R7). Epic 2's agent hardcodes it for launch speed; AD-1/R7's row in the Capability map binds reviews to the `content` module, so epic 6's agent builds an editor for a row that was never seeded — and finds the page ignores it.

**Proposed closing rule (tighten the Data convention):**
> Convention (added): All home-page section keys are defined in a single section-key registry created and owned by the home-trust epic and seeded into the `content` module at that epic's build (including the contact section shell and its editable copy). Later epics reference existing keys; adding a key requires extending the registry, never creating an ad-hoc row. The reviews message and the guarantee copy are `content`-module rows seeded by epic 2 and edited by epic 6; no visitor-visible copy on the home scroll may be hardcoded in `web/`.

## F8 — The Project JSON contract: two serializers, one resource, plus slug mutability and screenshot URL shape (project-proof vs admin)

**Pair:** epic 3 vs epic 6

**Both-compliant readings that collide:**

- Epic 3's agent defines the **public read model**: `GET /api/projects/` returns technologies as an array of strings, screenshots as whatever the DB stores, `translation_line` tolerated as nullable (its seed data is hand-made), slug as URL identity. Compliant with the conventions.
- Epic 6's agent defines the **admin write model** independently: technologies as objects `{name, slug}`, screenshots as storage-relative IDs (AD-8 only says "Django stores only URLs" — an agent can read "URL" loosely), slug editable (nothing says it isn't), figures as nested objects with client-generated ids. Compliant too.
- Assembly result: the gallery renders technologies as strings while the admin stores objects; screenshots that don't render in `next/image` (remote-patterns config mismatch); an admin slug edit silently breaks every published `/projects/[slug]` URL and leaves stale revalidated pages.

**Proposed closing rule (tighten the Data conventions):**
> Convention (added): One canonical Project JSON schema lives in the Django repo (serializer + contract snapshot test) and is owned by the project-proof epic; admin endpoints consume and produce the same representation, extended — never forked — by the admin epic. Fields: technologies are objects `{name, slug}`; every screenshot is an absolute `https://` URL directly renderable by the public pages; `translation_line` is non-nullable end-to-end (RULE-1 by construction, no dev-only nulls). Project slugs are immutable after creation; editing a display name never changes the URL.

## F9 — Object storage is deferred past the epic that needs it, and the upload path is unowned (project-proof vs admin vs foundation)

**Pair:** epic 3 vs epic 6 (vs epic 1)

**Both-compliant readings that collide:**

- The Deferred table pushes the media-storage vendor to the **admin epic**. But epic 3 runs *before* epic 6 and its done-when requires a launch project **with screenshots** hosted durably (AD-8 forbids local disk). Epic 3's agent must either pick a storage vendor itself (violating the deferral) or fake it (violating its done-when at launch when the fakes break). Compliant-by-the-letter deadlock.
- Upload path unowned: AD-8 binds admin, project-proof, foundation, but says nothing about the route a screenshot takes. Epic 6's agent may build (a) multipart upload through Django → storage (server holds credentials — consistent with "admin module is the only writer") or (b) **presigned direct upload SPA → storage** (faster; the SPA briefly holds storage credentials — an agent can argue AD-2 governs the *database*, not storage). Both defensible, incompatible.
- Size/limits: no max upload size anywhere; a 50 MB screenshot through a small Python host is a launch-epic surprise.

**Proposed closing rule (amend AD-8 + Deferred):**
> AD-8 (amended): Object storage is provisioned as part of the foundation scaffold (an `object-storage` port in `integrations` with a local/dev adapter); the admin epic picks the vendor and fills credentials into the existing port — the deferral covers the vendor choice only, and it must be resolved before epic 3's content seed. Screenshots are stored as absolute public URLs; uploads go multipart through the Django admin module only (the SPA never holds storage credentials); max upload size is fixed at scaffold time (default 10 MB) and enforced at the API layer.

## F10 — The contact target email: env config vs DB row, and who seeds it (contact-form vs admin)

**Pair:** epic 4 vs epic 6

**Both-compliant readings that collide:**

- The conventions say "Config and secrets via environment variables, never committed." Epic 4's agent classifies the recipient address as config and reads `CONTACT_EMAIL` from the env. Compliant.
- The ERD shows `CONTACT_SETTINGS ||--|| MESSAGE` and epic 6's done-when says "the contact target email … update[s] through the admin area." Epic 6's agent builds a DB-backed settings editor. Compliant.
- Sub-clashes: who creates the `CONTACT_SETTINGS` row (epic 6's edit endpoint assumes it exists; epic 4 assumes it will be created later — the first form submission before any admin visit then fails or falls back silently); what happens when the row is missing (hard fail vs env fallback — two different failure modes chosen by two agents).

**Proposed closing rule (amend AD-1 wording + conventions):**
> Convention (added): The contact target email lives **only** in the database (`CONTACT_SETTINGS`, single row), seeded by a migration owned by the contact-form epic; environment variables hold only SMTP credentials and secrets, never editable business config. Epic 4 owns the seed and the read; epic 6 owns the edit endpoint. A missing row is a startup error in Django, never a silent fallback address.

## Minor conventions gaps (batched)

**Pair:** epic 4 vs epic 5, epic 1 vs all.

- **Error-code registry:** the envelope `{error: {code, message}}` fixes the shape but not the codes. Epic 4 invents `captcha_failed`; epic 5 invents `guard_blocked` for the same condition; epic 3 invents `not_found` vs epic 2's `missing_resource`. Closing rule: a single error-code registry (constant table in the Django repo, mirrored in `web/`) owned by the foundation epic; new codes are added there, never inline.
- **Analytics conversion events:** epic 1 "records conversions", epics 4 and 5 "land conversion events". Event names and emission point (SPA vs API) are undefined; two agents will emit differently-named duplicates. Closing rule: analytics event names are defined by the foundation epic (`conversion_contact`, `conversion_booking`), emitted from the SPA at the confirmation screens, via one helper epic 1 ships.
- **Env-var registry:** at least nine env vars are implied across epics (API base URL, CORS origins, CSRF origins, revalidation token, captcha keys, SMTP credentials, storage credentials, DB URL, secret key). Closing rule: the foundation epic owns a single `.env.example` listing every var with owner epic and consumer deployable; epics extend it, never shadow it.

---

## Summary table

| # | Pair | Hole | Severity | Closing rule |
|---|------|------|----------|--------------|
| F1 | contact ↔ admin | AD-2 "one writer: admin module" vs `messaging/` module & public intake write | **Blocking** | Amend AD-2: per-module table ownership; public surface has exactly one write endpoint |
| F2 | contact ↔ booking ↔ admin | Booking notification channel contradiction; delivery-status shape/owner undefined | **Blocking** | Amend AD-7: enum field on Message, owned by messaging; booking notifications via email port |
| F3 | foundation ↔ admin | Revalidation webhook halves both unbuilt / both built; page-path map unowned | **Blocking** | Amend AD-10: foundation builds both halves + path map; admin only calls |
| F4 | contact ↔ booking | Captcha placement vs third-party scheduler bypass; site-key distribution unowned; rogue public write path risk | **Blocking** | Amend AD-9: Django-side verify + signed booking handoff; config endpoint owned by foundation |
| F5 | foundation ↔ admin ↔ launch | Login endpoint shape, CSRF trusted origins, CORS-with-credentials unowned | **Blocking** | Amend AD-6: foundation owns `/api/auth/*`, CORS/CSRF env config; built-in admin disabled |
| F6 | home/proof ↔ launch | SSR fetch timeout, degradation rule, warm-API mechanism unowned | High | New AD-11: shared fetch layer, 3 s budget, stale-serve rule; warmth owned by epic 7 |
| F7 | home ↔ contact ↔ admin | Duplicate section-text keys for the contact section; hardcoded vs editable reviews message | High | Convention: section-key registry owned by epic 2; no hardcoded visitor copy |
| F8 | proof ↔ admin | Two independent Project serializers; screenshot URL shape; slug mutability breaks URLs | **Blocking** | Convention: one canonical schema + contract test; absolute URLs; slugs immutable |
| F9 | proof ↔ admin ↔ foundation | Storage vendor deferred past the epic that needs it; upload path and size unowned | High | Amend AD-8: port provisioned at foundation; vendor filled by epic 6 before epic 3 seed; multipart-only upload |
| F10 | contact ↔ admin | Target email as env var vs DB row; seed and missing-row behavior undefined | Medium | Convention: email only in DB, seeded by epic 4; env holds credentials only |
| M1 | contact ↔ booking / foundation | Error-code names, analytics event names, env-var registry | Medium | Foundation-owned registries; batched convention additions |

**Bottom line:** the spine's invariants are sound; its failure mode is *ownership silence* — nearly every hole is a thing two epics were each allowed to assume the other built, or a data shape two epics were each allowed to invent. The ten closing rules above convert each silent assumption into a named owner and a named artifact, which is exactly what independent per-epic agents need.
