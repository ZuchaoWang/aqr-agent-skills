# Code and test style (JavaScript)

Opinionated JavaScript-stack style defaults. Apply these unless the project records a different choice.

## 1. Naming

- Files: `kebab-case.js` (`.ts`, `.jsx`, `.tsx`); `PascalCase` for component files.
- Functions and variables: `camelCase`.
- Private members: `_camelCase` (leading underscore by convention).
- Classes and components: `PascalCase`.

## 2. Project files and editor config

- `.nvmrc` — pins the Node version (single line, no `v` prefix, e.g. `20.18.0`); read by version managers (nvm, fnm, etc.).
- `.editorconfig` for JavaScript (`[*.{js,ts,jsx,tsx}]`): `indent_style = space`, `indent_size = 2`, `end_of_line = lf`, `charset = utf-8`, `insert_final_newline = true`, `trim_trailing_whitespace = true`. Line length is governed by the formatter (e.g. Prettier `printWidth`), not `.editorconfig`.
