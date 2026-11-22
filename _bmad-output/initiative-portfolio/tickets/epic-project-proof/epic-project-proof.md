---
id: 3
type: epic
title: "Project proof pages"
parent: initiative-portfolio
covers: ["R2", "R3", "R9"]
sources: []
companions: ["_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md"]
after: ["epic-home-trust"]
refined: false
risk: low
---

# Project proof pages

## Description

Claims become checkable evidence: gallery tiles open project pages carrying scale figures with plain-language translation lines, the technologies behind the system, the live URL when one exists, screenshots, and the public GitHub repo link — or the stated reason when the code is not publishable. The launch gallery holds one real project (R9's launch part; its growth part is epic 6's).

## Outcome

A technical evaluator judges the code without a call, and a non-technical visitor reads each figure's meaning for their own situation.

## Done when

- A gallery tile opens its project page.
- Every scale figure shown is paired with its translation line.
- A project page shows figures, technologies, live URL when one exists, screenshots, and the repo link — or states why the code is not publishable.
- The launch gallery carries at least one real project.

## Boundaries

- Belongs to siblings: growing the gallery through the admin area (epic 6), the home page shell (epic 2).
- Depended on by: epic 6 extends this epic's canonical Project schema — never forks it; epic 7 re-verifies these proof pages in production.
- Touch point: Hamza's public GitHub repos — linked from project pages; the content (figures, screenshots, repo) comes from Hamza.

## References

- parent — `_bmad-output/initiative-portfolio/initiative-portfolio.md`, Requirements R2, R3, R9
- prd — `_bmad-output/initiative-portfolio/prd-portfolio/prd-portfolio.md`, FR-2, FR-3, FR-4, FR-14, UJ-1, UJ-2
- companion — `_bmad-output/initiative-portfolio/architecture-portfolio/architecture-portfolio.md`, spine for this epic: AD-1, AD-4, AD-8 (consumes the object-storage port's absolute `https://` URLs; the launch seed runs on epic 1's dev/local adapter — the vendor swap is epic 6's, verified in production at epic 7, per the spine's amended AD-8), AD-10 (extends the revalidation path map for project pages), AD-11; convention: this epic owns the one canonical Project JSON schema (serializer + contract snapshot test) that epic 6 extends, never forks; scale figures are `(value, unit, translation_line)` triples by construction (RULE-1), and a project carries `repo_url` or a non-empty `no_repo_reason` (validation-enforced, FR-4)

## Notes

- Waits on epic-home-trust because: it builds into the shell and gallery slot that epic provides.
- The launch project's figures, screenshots and repo are content Hamza provides — a person's part in this epic.
- Decision: 2026-11-22 — the launch project's screenshots are stored through epic 1's dev/local storage adapter (the AD-8 port); epic 6's vendor pick migrates the stored content, and epic 7 verifies the swap in production. Resolves the spine AD-8 contradiction the tree check found.
- Assumption: this epic's revalidation path-map extensions (AD-10) are additive, keyed `project:<slug>` entries that no other epic writes, so no ordering against epic 4 is needed; a merge collision, should one appear, is resolved at merge time.
