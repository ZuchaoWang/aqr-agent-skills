# Documentation index

This is the authoritative documentation map for the project. New readers start here. Update this file whenever a doc is added, removed, or moves between sections.

## Top level

- `project_layout.md` — top-level repo layout and module-doc criteria.

## Project

- `project/mission.md` — {{mission, problem statement, scope}}.
- `project/roadmap.md` — {{milestones, dates, owners}}.

## Architecture

- `architecture/design.md` — {{system-level design, module boundaries, data flow}}.

## Implementation

- {{one entry per module folder under `implementation/` (path: `{{module_name}}/` or `{{layer}}/{{module_name}}/`), with a one-line role}}.

## Research

- {{one entry per `research/<topic>.md`, with a one-line takeaway}}.

## Data

- {{one entry per `data/<dataset>.md`, with source and refresh cadence}}.

## Rules

- `rules/doc_markdown.md` — markdown doc style: headings, lists, lifecycle, archival.
- `rules/frontend_js.md` — JS / TS frontend rules; include only if the project has a frontend.
- `rules/backend_python.md` — Python backend rules; include only if the project has a Python backend.
- `rules/backend_jupyter.md` — Jupyter backend rules; include only if the project ships notebooks as a backend.
- `rules/report_ppt.md` — PowerPoint report conventions; include only if the project ships decks.
