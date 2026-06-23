# {{project-name}} — Agent instructions

{{One-paragraph elevator pitch. State what the project does and point at `docs/index.md` as the documentation entry point.}}

## How to work in this repo

Project documentation starts at `docs/index.md`. Layout reference: `docs/project_layout.md`.

When modifying code, follow `docs/rules/code_general.md` plus the per-stack rule files that apply (e.g. `frontend_js.md`, `backend_python.md`, `backend_jupyter.md`).

When updating documentation, follow `docs/rules/doc_markdown.md` for format and `docs/rules/doc_templates.md` for content criteria on dynamic doc types.

Issue records (if the project uses `aqr-issue-records`) live under `issues/`, grouped by `YYYYMM`.

## Toolchain

{{Runtime / toolchain specifics. One bullet per runtime. Include language version, where the version file lives, anything that surprises a fresh agent (e.g. system Node is the wrong version; pyenv env name).}}

- {{Language version — see `.python-version` / `.nvmrc` / equivalent.}}
- {{Other relevant toolchain notes.}}
