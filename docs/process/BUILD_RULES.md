# Hertect Build Rules

Applies to every agent session on this repo, Codex or Claude Code. Read this before any work. If AGENTS.md and this file disagree, this file wins for process and AGENTS.md wins for scope.

Context: Ctrl Alt Delegate is built by one engineer (Heather) and coding agents. There is no review team, no PR checklist, no approval queue. The rules below exist so that one person can trust what the agents ship.

## 1. Read first

Read AGENTS.md, then this file, then the current stage in docs/product/BUILD_PLAN.md. Do not start work without knowing which stage is open and who owns the seam (see rule 6).

## 2. Definition of done

Done means a named user workflow works end to end on the deployed develop Preview, against the live database, and survives a page refresh.

Report done with the Preview URL and a screenshot of the workflow completing.

Fixture data is never evidence of done. Screens that render fixtures with no persistence are not done.

## 3. Vertical slice before any split

Before work on a feature is split across sessions or agents, one session builds a thin vertical slice: source, database, read model, screen, record. That slice must be on the develop Preview per rule 2. Only then may UI and backend work run in parallel.

## 4. Keep going

Once work is authorized, continue through the stage without asking for confirmation on each step. Migrations are self reviewed and applied to the develop Preview. PRs merge to develop on green checks without waiting for Heather.

Stop and report only for:

- Anything touching main, Production, HER, or Playdentity
- Any repo other than the two synthetic fixture repos
- A canary secret or credential exposure
- A conflict between the spec and the fixtures
- Gate M2 or any stage exit

Never merge to main. Heather promotes to Production.

## 5. No side quests

Do the authorized work only. No refactors, dependency upgrades, tooling changes, or "while I was in there" fixes unless they block the authorized work. If you find something that should be fixed, write one line in the stage exit report.

## 6. One owner of the seam

For each run, one named session owns the seam: database, lib/server, workers, and the wiring between them. Any other session in the same run owns app and components only and does not touch the seam. The current seam owner is recorded in AGENTS.md under Current run and is updated at stage exits.

## 7. Short reports

Reports happen at stage exits and stops only. No daily reports. No evidence packages for feature PRs.

A stage exit report is:

- What shipped, with Preview URL and screenshot
- What did not ship and why
- One line per side quest not taken
- The next stage to open

## 8. Design review on request only

Heather may say "design review." Only then convene the four seat design review board (architecture, data, security, product) and write the outcome as an ADR in docs/decisions/. Do not run design reviews unprompted.

## 9. Product truth lives in the repo

Positioning, vocabulary, and banned claims come from docs/product/PRODUCT.md and docs/positioning/CAD_COPY.md. Feature scope comes from docs/product/FEATURE_INVENTORY.md. Sequence comes from docs/product/BUILD_PLAN.md. Decisions come from docs/decisions/. Nothing in a chat window overrides these files. If a chat instruction changes one of them, update the file in the same PR.
