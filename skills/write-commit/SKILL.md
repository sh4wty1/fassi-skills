---
name: write-commit
description: Write a git commit message in the convention the repository already uses, then commit the staged changes once the user approves. Use whenever you are about to create a commit or are asked for a commit message.
---

Write the message for the staged changes in the repository's own **convention**, and commit once the user approves.

## 1. Find the convention

The first of these that exists wins:

1. **Written rules**: a commitlint config (`commitlint.config.*`, `.commitlintrc*`, a `commitlint` key in `package.json`), or a section on commits in `CONTRIBUTING*`, `AGENTS.md` or `CLAUDE.md`.
2. **History**: the subjects of `git log -20 --no-merges --format=%s`. When most of them share a format, follow it in every respect: prefix or none, language, capitalisation, tense, length.
3. **Fallback**: with fewer than 5 commits, or no format that most of them share, use the fallback below.

## 2. Fallback convention

Conventional Commits, in English:

```
<type>(<scope>)!: <subject>

<body>

<footer>
```

| Type | For |
| --- | --- |
| `feat` | New behaviour for the user |
| `fix` | A bug corrected |
| `docs` | Documentation only |
| `refactor` | Code restructured, behaviour unchanged |
| `test` | Tests added or corrected |
| `perf` | Same behaviour, faster or lighter |
| `build` | Build system or dependencies |
| `ci` | CI configuration |
| `chore` | Maintenance that fits no other type |
| `revert` | A previous commit undone |

- **Scope**: optional, the module touched, when the repo has clear modules.
- **Breaking change**: `!` before the colon, plus a `BREAKING CHANGE:` footer saying what breaks.
- **Subject**: imperative, lower-case start, no final period, at most 72 characters counting the prefix.
- **Body**: only when the reason is not obvious from the diff. It says why, wrapped at 72 columns.
- No emoji.

## 3. Read the staged changes

Read `git diff --staged`.

- **Nothing staged**: list the changed files from `git status --short` and ask which ones enter. Stage only the files the user names.
- **Unrelated changes staged together** (the subject would need an "and"): say so and propose the split, with the files and the message of each commit. Split only when the user accepts; otherwise write one commit.

## 4. Propose, then commit

Show the message in a code block, with one line saying which convention it follows and where that came from (the rules file, the history, or the fallback). Commit when the user approves, after applying any change they ask for. An instruction to commit without asking, given earlier in the session, counts as approval.

Add the trailers the repo's rules or your own instructions require, and no others.

Leave these to an explicit request: staging everything (`git add -A`, `git add .`), `--amend`, `--no-verify` and `git push`. When a hook rejects the commit, report its output and fix the cause.
