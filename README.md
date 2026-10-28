# brainroot-skill

Personal Claude Code skills.

## Skills

| Skill | What it does | Start it with |
|---|---|---|
| `helicopter` | Runs a project as small, user-approved tasks. Every step is tracked, explained before it starts, and proven with evidence after it ends. | `/helicopter` |

## Install

Link a skill into your Claude Code skills folder:

```sh
ln -s "$(pwd)/skills/helicopter" ~/.claude/skills/helicopter
```

Run the command from the repo root. Start a new Claude Code session afterwards.

## Dependencies

`helicopter` calls two other skills. It still works without them, with a simpler fallback.

- `junior-change-doc` — writes the overview HTML page.
- `mattpocock-skills:grilling` — interviews you about each task. Part of the `mattpocock-skills` plugin.
