# Ctrl Alt Delegate Action: build brief

Date: September 16, 2026
Repo: Hertect-Consulting/ctrl-alt-delegate-action (public)
Branch pattern: Direct. Commit to main. GitHub auto merge on PRs opened by agents.
Owner: one Claude Code session, its own worktree. No second session.
Rules: docs/process/BUILD_RULES.md (copy from Hertect-Consulting/.github), AGENTS.md pointing at it, one line CLAUDE.md.

## What it is

A GitHub Action that reads the instruction files AI coding agents follow in one repo and fails the pull request when they are wrong. One PR comment. Never edits a file. Signed Ctrl Alt Delegate.

## Scope, and nothing else

Discovery, at the PR head commit:
- AGENTS.md (root and nested)
- CLAUDE.md, following one level of @import
- .github/copilot-instructions.md
- .cursor/rules/**/*.mdc and .cursorrules
- GEMINI.md
- **/SKILL.md

Rule parsing: a rule is a block that carries the five line shape (What, Applies to, Check, Owner, Reviewed) or a top level heading block in a file that does not use the shape. Skills are one rule each.

Four checks, each a finding with file, line, and one plain sentence:
1. Dead command. A backticked command in a Check or What line whose first token is a package.json script, Makefile target, or a binary named in a CI workflow, and none of those define it. Do not flag shell builtins or common tools (git, npm, npx, pnpm, yarn, make, go, cargo, python, pip). Keep that allow list in one file.
2. Dead path. A repository relative path in Applies to, What, or Check that does not exist at the head commit. Globs are expanded; a glob matching zero files is a finding.
3. No Check line. A rule in five line shape with an empty or missing Check, or a heading block rule with no backticked command and no path anywhere in it.
4. Contradiction, v1 narrow: two files declare a Check for the same purpose (test, lint, build, typecheck, keyed by the script name family) with different commands, or two files state a different version for the same tool (Node, Python, Go) as a bare version string. Nothing semantic.

Output:
- One sticky PR comment, updated on each run, not a new comment per run. Grouped by check. Each finding: file:line, sentence, and for dead command or path what was looked for.
- Check conclusion: failure if any finding, success if none. Input `fail-on` (default `all`, options `none`, `dead-only`) so a repo can start in report only mode.
- Last line of the comment, always: "Receipts for one repo. Ctrl Alt Delegate keeps them for all of yours." with a link to hertect.com/ctrl-alt-delegate.
- Also emit a JSON summary as a workflow output for anyone who wants it.

Inputs: `paths` (extra instruction file globs), `fail-on`, `allow-commands` (extra allow list). No tokens beyond GITHUB_TOKEN with pull-requests: write, contents: read.

Never: write to the repo, open PRs, rewrite rules, send data anywhere, look at other repos, keep state between runs.

## Build order

1. Repo skeleton: action.yml, TypeScript on Node 20, only @actions/core and @actions/github as runtime dependencies, bundled with ncc, dist committed. Tests with vitest. Fixture repos under test/fixtures, one per check plus one clean repo.
2. Discovery and parsing with fixtures. Exit: parser tests green on all five file kinds and on the five templates.
3. Checks 1 to 3. Exit: each fires on its fixture and stays silent on the clean fixture.
4. Check 4, narrow as written. Exit: fires on the two contradiction fixtures only.
5. PR comment and check conclusion. Exit: run on a PR in this repo and screenshot the comment.
6. Templates folder: the five templates, rewritten so each carries one specific example rule and a blank block, and so the Action passes on every one of them. README leads with the five line shape, then install, then the templates. LICENSE: MIT for the code, CC BY 4.0 for the templates folder.
7. Dogfood: add the workflow to ctrl-alt-delegate, hertect, her. Loud Silences is not a dogfood repo and must never be. Fix any finding in those repos' own instruction files or record why it stays. Exit: green on all three, screenshots.
8. Tag v1.0.0 and v1. Publish to the GitHub Marketplace under the name Ctrl Alt Delegate with the approved description. Report the Marketplace URL.

Done means step 7 evidence and the Marketplace listing live. Three days.

## Copy it carries

Marketplace name: Ctrl Alt Delegate
Marketplace description: Checks the instruction files your AI coding agents follow and fails the pull request when they're wrong. Free for any repo.
Workflow usage: uses: Hertect-Consulting/ctrl-alt-delegate-action@v1
Word rules from docs/positioning/CAD_COPY.md apply. "Instruction files" in every sentence; "manual" banned; full name first.
