# aqr-agent-skills

Custom, opinionated AI-coding skills. Source repository — install by copying or symlinking a skill directory into the host agent's skills folder (e.g. `~/.claude/skills/` for Claude Code).

## Skills

- `aqr-doc-blueprint` — Lay out or compare a project's docs against the recommended `docs/` tree. Auto-invocable; docs layout.
- `aqr-doc-criteria` — Content criteria for common documentation types (design, interface, project, research, dataset, milestone). Auto-invocable; applied when writing or reviewing docs.
- `aqr-code-criteria` — Universal code quality principles that apply regardless of language or stack. Auto-invocable; applied when writing or reviewing code.
- `aqr-project-principle` — Working principles for any project work: clarify before committing, documentation as project memory, optimize for human review, keep the solution simple, verify before completion. Auto-invocable; applied during work.
- `aqr-style-rules` — Opinionated, stack-specific style defaults for code, tests, notebooks, presentations, and doc formatting. Auto-invocable; applied when writing or reviewing code, tests, notebooks, or decks.

All skills are doc only — no executable code.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition
  reference/            # explanatory docs loaded on demand
```

See `CLAUDE.md` for editing and installation details.

## References

External sources adapted by `aqr-doc-criteria` and `aqr-code-criteria`:

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) (M. Nygard, 2011) — the Architecture Decision Record format adapted in `reference/design.md` §2.4.
- [architecture-decision-record](https://github.com/joelparkerhenderson/architecture-decision-record) (J. P. Henderson) — ADR templates and writing guidance.
- *Visualization Analysis and Design* (T. Munzner, CRC Press) — the marks-and-channels, expressiveness, and effectiveness principles adapted in `reference/design.md` §2.3.
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) (D. Abramov) — the presentational/container distinction adapted in `reference/design.md` §2.2.
- [Software Architecture skill](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/skills/development/software-architecture/SKILL.md) (claude-code-templates) — the library-first / NIH guidance and code-quality rules adapted in `aqr-code-criteria` and `reference/design.md` §2.1.
