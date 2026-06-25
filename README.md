# aqr-agent-skills

Custom, opinionated AI-coding skills. Source repository — install by copying or symlinking a skill directory into the host agent's skills folder (e.g. `~/.claude/skills/` for Claude Code).

## Skills

- `aqr-project-blueprint` — Scaffold or compare a project against the recommended project/doc/code convention blueprint. Manual-only; project shape and conventions, independent of any issue workflow.
- `aqr-content-criteria` — Content criteria for common documentation types (design, interface, usage scenarios, project, research, dataset) and source code. Auto-invocable; applied when writing or reviewing doc or code.
- `aqr-issue-records` — Lightweight repo-visible issue records: `task.md` and `progress.md` plus one completion artifact (`summary.md` for a change, `report.md` for an investigation) per issue under `issues/<YYYY-MM-DD>-<name>/`. No `plan.md`, no issue types, no issue groups. Composes with execution methodology skills (e.g. Superpowers) but is not tied to any one.

All skills are doc only — no executable code.

## Issue records philosophy

`aqr-issue-records` is deliberately thin. Three things have different lifetimes and should not be conflated:

- **Project docs are durable.** They live under `docs/` and are the real review artifacts.
- **Issue docs are disposable coordination.** They exist only to help recovery, review, or accountability — not to capture every agent thought.
- **Agent plans are temporary cognition.** Planning is an agent behavior, not a repo artifact.

So the skill ships no `plan.md`: the agent plans privately, and the repo holds only what a human would actually read — what was asked, what was decided, what shipped. You do not need to review an agent's plan document when the real review artifact is the updated documentation. Issue files should exist only when they help recovery, review, or accountability; for small changes, no issue is needed at all.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition
  templates/            # starting-point files copied into target projects
  reference/            # explanatory docs loaded on demand
```

See `CLAUDE.md` for editing and installation details.

## References

External sources adapted by `aqr-content-criteria`:

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) (M. Nygard, 2011) — the Architecture Decision Record format adapted in `reference/design.md` §2.4 and `reference/interface.md`.
- [architecture-decision-record](https://github.com/joelparkerhenderson/architecture-decision-record) (J. P. Henderson) — ADR templates and writing guidance.
- *Visualization Analysis and Design* (T. Munzner, CRC Press) — the marks-and-channels, expressiveness, and effectiveness principles adapted in `reference/design.md` §2.3.
- [Google API Improvement Proposals](https://google.aip.dev) — API design conventions (error envelope, versioning, pagination, idempotency) adapted in `reference/interface.md`.
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) (D. Abramov) — the presentational/container distinction adapted in `reference/design.md` §2.2.
- [Software Architecture skill](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/skills/development/software-architecture/SKILL.md) (claude-code-templates) — the library-first / NIH guidance and code-quality rules adapted in `reference/code-criteria.md` and `reference/design.md` §2.1.
