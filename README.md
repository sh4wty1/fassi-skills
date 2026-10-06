# fassi-skills

My personal registry of agent skills, and one command to install them into any project.

Most of the skills are third-party. The registry points at their sources instead of copying them, so they stay updatable and keep their own licenses.

## Install

Once per machine. Needs Node.js.

### Claude Code, as a plugin

```
/plugin marketplace add sh4wty1/fassi-skills
/plugin install fassi-skills@fassi-skills
```

The command is then `/fassi-skills:setup-fassi-skills`. Update with `/plugin marketplace update fassi-skills`.

### Any agent, with npx

Works for Claude Code, Cursor, Codex, Copilot, Windsurf and others.

```bash
npx skills@latest add sh4wty1/fassi-skills --skill setup-fassi-skills -g
```

The installer asks which agents to install it for. Add `-a <agent> -y` to skip the prompt, for example `-a claude-code -y`. The `setup-fassi-skills` skill is then available in every project, as `/setup-fassi-skills`. Update with `npx skills@latest update -g`.

## Use

Open your agent at the root of a project and run:

```
/setup-fassi-skills
```

With the plugin, that is `/fassi-skills:setup-fassi-skills`. In agents without slash commands, ask it to run the `setup-fassi-skills` skill.

It reads the project, recommends the skills that fit it, and asks how to proceed:

| Option | What happens |
| --- | --- |
| Install recommended | Installs the skills it recommended for this project |
| Install all | Installs every skill in the registry |
| Choose myself | Opens a picker grouped by category, recommendations marked |
| Help me decide | Asks what you are building, then opens the picker with new recommendations |

Skills are installed into the project, for the agent you ran it from; name other agents when it asks and it installs for those too. At the end it lists what was installed and what failed.

When the project needs something the registry does not cover, it suggests skills from outside, each with a link to review. For the ones you install, it prints the entry to add to the registry.

## What is in the registry

Everything lives in one file: [`skills/setup-fassi-skills/manifest.json`](skills/setup-fassi-skills/manifest.json).

| Section | Holds |
| --- | --- |
| `sources` | Where skills come from, their license and the install command, with `{skill}` as placeholder |
| `skills` | Name, source, category and when to use each skill |
| `workflows` | Skills that form a flow, in order, and when to pick that flow |
| `tools` | CLIs that are run, not installed |

Current sources: [mattpocock/skills](https://github.com/mattpocock/skills) and [tech-leads-club/agent-skills](https://github.com/tech-leads-club/agent-skills).

## Add a skill

Add one object to `skills` in the manifest:

```json
{
  "name": "tdd",
  "source": "mattpocock",
  "category": "quality",
  "when_to_use": "Build a feature or fix a bug test-first."
}
```

A skill from a new place also needs one entry in `sources`. The command itself never changes.

Then push, and update the installed copy as shown in [Install](#install).

## Skills written by me

They live in this repo as real folders, `skills/<name>/SKILL.md`, and are installed the same way as `setup-fassi-skills`.

| Skill | What it does |
| --- | --- |
| `help-me` | `/help-me I need to do X`: recommends the skills and development patterns to use for that task, in order |

## License

[MIT](LICENSE) for what is written here. Third-party skills keep the licenses of their sources.
