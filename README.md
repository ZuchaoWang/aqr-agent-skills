# aqr-agent-skills

Custom, opinionated AI-coding skills. Source repository — install by copying or symlinking a skill directory into the host agent's skills folder (e.g. `~/.claude/skills/` for Claude Code).

## Skills

- `aqr-project-blueprint` — Scaffold or compare a project against the recommended project/doc/code convention blueprint. Manual-only; project shape and conventions, independent of any issue workflow.
- `aqr-content-criteria` — Content criteria for common documentation types (design, interface, usage scenarios, project, research, dataset) and source code. Auto-invocable; applied when writing or reviewing doc or code.
- `aqr-issue-records` — Repo-visible issue record format with five artifacts (`task.md` / `plan.md` / `progress.md` / `summary.md` / `report.md`) and four issue types (doc-update, code-update, fix, investigate). Composes with execution methodology skills (e.g. Superpowers) but is not tied to any one.

All skills are doc only — no executable code.

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
