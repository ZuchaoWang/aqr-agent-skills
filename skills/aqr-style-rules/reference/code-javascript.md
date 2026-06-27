# Code and test style (JavaScript)

Opinionated JavaScript-stack style defaults, layered on top of the universal floor in the `aqr-code-criteria` skill. Apply these unless the project records a different choice. This is a minimal seed — grow it as JavaScript conventions are fixed. Anything not mentioned here follows `aqr-code-criteria`.

## 1. Project files and editor config

- `.nvmrc` — pins the Node version (single line, no `v` prefix, e.g. `20.18.0`); read by version managers (nvm, fnm, etc.).
- `.editorconfig` for JavaScript (`[*.{js,ts,jsx,tsx}]`): `indent_style = space`, `indent_size = 2`, `end_of_line = lf`, `charset = utf-8`, `insert_final_newline = true`, `trim_trailing_whitespace = true`. Line length is governed by the formatter (e.g. Prettier `printWidth`), not `.editorconfig`.
