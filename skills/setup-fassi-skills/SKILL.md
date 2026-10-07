---
name: setup-fassi-skills
description: Install my curated agent skills into the current project, with recommendations based on what the project is.
disable-model-invocation: true
---

Install skills from the **manifest**, the `manifest.json` beside this file, into the current project. The manifest is the single source of truth: every skill name, install command, category, workflow and tool comes from it.

## 1. Read the manifest and the project

Read the manifest. Then survey the project from its root: `README*`, `AGENTS.md` / `CLAUDE.md`, the package manifest, the top-level layout, and any `*/skills/` directory for skills already installed.

The project root is the nearest ancestor holding `.git` or `package.json`; one installer writes there, so run every command from it. When neither marker exists, say so and ask whether to continue in the current directory.

Done when you hold a **recommended** set: the skills whose `when_to_use` fits this project, one workflow chosen per `workflow_rule`, each with a one-line reason drawn from what you read. An empty or unreadable project yields an empty recommended set.

## 2. Ask how to proceed

Show the recommended set with its reasons, then ask one question with these options:

- **Install recommended**: the recommended set. Offer it first, and only when the set is non-empty.
- **Install all**: every skill in the manifest. Put the count in the option, since each installed skill adds its description to every session.
- **Choose myself**: go to the picker.
- **Help me decide**: ask in plain text what they are building, plus at most two follow-ups (new or existing project, feature size, whether PRs are reviewed on GitHub). Rebuild the recommended set from the answers, show it, and go to the picker.

Ask with your structured question tool when you have one (`AskUserQuestion` in Claude Code); otherwise print a numbered list and read the numbers back.

## 3. Picker

First ask which categories to browse: one multi-select whose options are the manifest categories, each with its skill count, suffixed `(Recommended)` when it holds a recommended skill. Then one multi-select question per chosen category. Each option is a skill: its name as the label, its `when_to_use` as the description. Suffix the label with `(Recommended)` for the recommended set and `(installed)` for skills already present.

`AskUserQuestion` takes 1-4 questions per call and 2-4 options per question, so:

- the category list, and a category with more than 4 skills, split into `<category> (1/2)`, `(2/2)`;
- a category with 1 skill merges into its nearest neighbour;
- more than 4 questions continue in a second call.

### Suggestions from outside the manifest

When the project needs something no manifest skill covers, look for a **suggestion**: first among the other skills of each source's `repo`, then with `npx --yes skills@latest find <keyword>`. Keep at most 4, each one read (its `SKILL.md`) before you offer it.

Suggestions get their own picker question, `suggestions`, with the repo URL in each description so the user can review it. They are installed only when picked there: "Install recommended" and "Install all" cover the manifest alone.

## 4. Install

The target **agents** default to the agent running this skill. Mention that default in one line with the question in step 2, so the user can name others.

For each chosen skill, in manifest order, one at a time: take its source's `install` command (for a suggestion from an unlisted repo, `any_repo_install` with `{repo}` as `owner/name`), replace `{skill}` with the skill name and `{agents}` with the space-separated agent ids, and run it from the project root. A skill marked `(installed)` is reinstalled only when the user picked it.

Agent ids are the installer's own, and the installers disagree on a few (`gemini-cli` / `gemini`, `kilo` / `kilocode`, `kiro-cli` / `kiro`). When an installer rejects an id, read the supported list in that source's `repo` and retry with its spelling.

A skill counts as installed when a `skills/<name>/SKILL.md` exists under an agent directory of the project afterwards (`.claude/skills/`, `.agents/skills/`, `.cursor/skills/`, ...). Judge by that file, since an installer can exit 0 having installed nothing. Keep going after a failure.

## 5. Report

A table with every chosen skill: `installed` with the path, `already present`, or `failed` with the last lines of the installer's output. Then, in a few lines:

- the workflow these skills form and the order to use them;
- each entry of `tools` with its `run` command;
- for each suggestion installed, the ready-to-paste `skills` entry (plus a `sources` entry when its repo is new) to add to `manifest.json` in https://github.com/sh4wty1/fassi-skills, so it becomes part of the registry;
- any need still uncovered.
