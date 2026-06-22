# aqr-agent-skills

Custom, opinionated AI-coding skills. Source repository — install by copying or symlinking a skill directory into the host agent's skills folder (e.g. `~/.claude/skills/` for Claude Code).

## Skills

- `aqr-project-blueprint` — Scaffold or compare a project against the recommended project/doc/code convention blueprint. Manual-only; project shape and conventions, independent of any issue workflow.
- `aqr-issue-records` — Repo-visible issue record format with five artifacts (`task.md` / `plan.md` / `progress.md` / `summary.md` / `report.md`) and four issue types (doc-update, code-update, fix, investigate). Composes with execution methodology skills (e.g. Superpowers) but is not tied to any one.

Both skills are doc only — no executable code.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition
  templates/            # starting-point files copied into target projects
  reference/            # explanatory docs loaded on demand
```

See `CLAUDE.md` for editing and installation details.
