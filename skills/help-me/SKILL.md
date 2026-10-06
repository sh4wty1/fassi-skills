---
name: help-me
description: Decide how to approach a task. Given "I need to do X", recommend the skills and development patterns to use, in order.
disable-model-invocation: true
---

The user has a task and wants to know **how to approach it**, not to have it done. Answer with one recommended path made of skills and development patterns. Start the work only when they say so.

## 1. Get the task

The task is the argument passed to this skill. With no argument, ask in one line what they need to do.

## 2. Gather the candidates

- **Available skills**: every skill this session already lists, by its name and description.
- **Registry skills**: the `skills`, `workflows`, `workflow_rule` and `tools` of the manifest, at `../setup-fassi-skills/manifest.json` relative to this file, or else at https://raw.githubusercontent.com/sh4wty1/fassi-skills/main/skills/setup-fassi-skills/manifest.json. When neither is reachable, carry on with the available skills alone and say so.
- **The project**: `AGENTS.md` / `CLAUDE.md` and the code the task touches, read only as far as needed to judge the task's size and what conventions already exist.

## 3. Size the task

Settle these from the task and the project. Ask the user only for what you could not settle and that would change the recommendation, in at most two questions.

| Question | Points to |
| --- | --- |
| Is what to build still open? | Interview and spec first (grilling, spec skills) before any code. |
| Is the behaviour clear and testable? | Test-first: one failing test, make it pass, refactor. |
| Is it something broken or slow? | Reproduce it first, then diagnose; no fix before a reproduction. |
| Is the doubt about how it should feel or look? | A throwaway prototype, discarded after it answers the question. |
| Does it span several modules or sessions? | A spec cut into tickets, one ticket per session. |
| Does it change a module's shape or interface? | Design the interface first, then move callers in small steps. |
| Is it hard to undo (migration, cutover, public API)? | Smallest reversible slice first, with a way back written down. |
| Is it small, clear and local? | No process: just do it, then review the diff. |

When a registry workflow fits, recommend that workflow whole and respect `workflow_rule`.

## 4. Answer

Keep it to what the user can act on:

1. **The path**: numbered steps in the order to do them. Each step names a skill (as the command to type) or a pattern, with one line on why it fits *this* task.
2. **Not installed**: mark any recommended skill that is not available in this session, and point to `/setup-fassi-skills` to install it.
3. **The alternative**: one other path and the condition under which it would be the better choice.
4. **What to skip**: process that would be overkill for this task, in one line.

Recommend one path. When no skill fits a step, name the pattern alone rather than stretching a skill to cover it.

End by offering to start on step 1.
