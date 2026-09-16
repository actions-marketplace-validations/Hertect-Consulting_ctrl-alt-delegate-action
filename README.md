# Ctrl Alt Delegate

[![CI](https://github.com/Hertect-Consulting/ctrl-alt-delegate-action/actions/workflows/ci.yml/badge.svg)](https://github.com/Hertect-Consulting/ctrl-alt-delegate-action/actions/workflows/ci.yml)

Checks the instruction files your AI coding agents follow and fails the pull request when they're wrong. Free for any repo.

## The five line shape

A rule your agents can actually be held to has five lines:

```
What: One plain sentence.
Applies to: A path or glob.
Check: A command that passes, or something visible in the PR.
Owner: @handle
Reviewed: YYYY-MM-DD
```

If you can't write the Check line, the rule is a wish, not a rule. Ctrl Alt Delegate reads every instruction file your agents follow — AGENTS.md, CLAUDE.md, `.github/copilot-instructions.md`, Cursor rules, GEMINI.md, and SKILL.md files — at the pull request's head commit, and fails the check when a rule:

1. **Names a dead command.** The Check or What line backticks a command whose first token isn't git, npm, or another common tool, and isn't a package.json script, Makefile target, or binary named in a CI workflow.
2. **Points at a dead path.** Applies to, What, or Check names a repo-relative path or glob that matches nothing at the head commit.
3. **Has no Check line.** Five-line shape with an empty or missing Check, or a heading-style rule with no backticked command and no path anywhere in it.
4. **Contradicts another file.** Two files declare a Check for the same purpose (test, lint, build, typecheck) with different commands, or state a different version for the same tool (Node, Python, Go).

One sticky PR comment, updated on each run — never a new comment per run, never a rewrite of your files. Findings only.

## Install

```yaml
# .github/workflows/ctrl-alt-delegate.yml
name: Ctrl Alt Delegate
on: pull_request
permissions:
  pull-requests: write
  contents: read
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Hertect-Consulting/ctrl-alt-delegate-action@v1
```

### Inputs

| Input | Default | What it does |
|---|---|---|
| `paths` | *(none)* | Extra instruction file globs to discover, comma or newline separated. |
| `fail-on` | `all` | `all`, `none`, or `dead-only` (dead command + dead path only). Start a repo on `none` or `dead-only` to see findings without going red. |
| `allow-commands` | *(none)* | Extra command names to allow in Check/What lines, beyond the built-in allow list. |
| `token` | `github.token` | Token used to post the sticky PR comment. |

### Outputs

| Output | What it is |
|---|---|
| `findings-json` | Every finding, as JSON. |
| `finding-count` | Total finding count. |
| `conclusion` | `success` or `failure`. |

Never: writes to your repo, opens PRs, rewrites your rules, sends your data anywhere, looks at other repos, or keeps state between runs.

### If your org restricts Actions to verified creators

If your GitHub organization's Actions policy is set to allow only GitHub-verified creators (or a selected list), add `Hertect-Consulting/ctrl-alt-delegate-action` to that allow list before the workflow above will run. Organization Settings → Actions → General → Policies.

### Pull requests from forks

A `pull_request` run triggered by a fork gets a read-only `GITHUB_TOKEN` — GitHub does not grant it permission to post a PR comment. Ctrl Alt Delegate detects this and writes its findings to the job summary instead (visible on the workflow run page), rather than failing to comment or failing the job over it. Same-repo PRs still get the sticky PR comment as normal.

## Templates

`templates/` has one starter per instruction-file kind — AGENTS.md, CLAUDE.md, `.github/copilot-instructions.md`, a Cursor `.mdc` rule, and a SKILL.md — each with one worked example rule in the five line shape and a blank block to fill in. Every one of them passes Ctrl Alt Delegate as shipped. Copy the one you need into place and edit it.

The code in this repository is MIT licensed. The templates in `templates/` are CC BY 4.0 — see [templates/LICENSE](templates/LICENSE).
