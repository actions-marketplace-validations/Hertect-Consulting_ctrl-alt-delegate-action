# AGENTS.md

Read this first in every session. It overrides anything you infer from other files.

## What this repository is

ctrl-alt-delegate-action. The GitHub Action that reads the instruction files AI coding agents follow in one repo and fails the pull request when they are wrong. One PR comment. Never edits a file. Signed Ctrl Alt Delegate. Owner: Heather. See `action.yml` for inputs and outputs.

## Controlling documents

1. `docs/process/BUILD_RULES.md` controls process. Read it before any work.
2. `docs/BUILD_BRIEF.md` controls scope, the four checks, and build order. Follow the order and its exit criteria; stop only where it says to stop.

If BUILD_RULES.md and this file disagree, BUILD_RULES.md wins for process and this file wins for scope, per BUILD_RULES.md rule 1.

## Ownership

One Claude Code session, its own worktree, owns this repo. No second session. See `docs/BUILD_BRIEF.md` line 6.

## Current run

- 2026-09-16: session executed docs/BUILD_BRIEF.md build order through step 7 (workflow rollout to ctrl-alt-delegate, hertect, her — Loud Silences is not a dogfood repo and must never be). Stopped before step 8 (tag v1.0.0/v1 + Marketplace publish) for a short report.
- No further PRs on this repo for the rest of 2026-09-16. Two small items are queued and must land together, not separately: the "Wrong? Tell us" feedback link and the issue templates. Open them as one PR titled "Add feedback link and issue templates," targeting v1.0.1, on 2026-09-17 or later.

## Boundaries

- The Action itself never writes to the repo it scans, opens PRs, rewrites rules, sends data anywhere, looks at other repos, or keeps state between runs — see docs/BUILD_BRIEF.md "Never" list.
- This session never touches main of another repo, Production, HER, or Playdentity.

## Commit and PR rules

This repository is public. PR titles and commit subjects describe the change in plain terms: what changed. Never the reason, the mistake being fixed, or the internal discussion that led to it. A reason, when one is worth recording, goes in the PR body, in one line.
