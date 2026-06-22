Status: placeholder

# Implementation overview

Per-module implementation reference. Each top-level directory or major module in the source tree gets a one-line role here and a dedicated per-module doc elsewhere under `implementation/`.

## 1. Module map

- `path/to/module` — {{one-line role}}. See `{{module-name}}.md`.

## 2. Conventions shared across modules

{{What every module agrees on: error handling, logging, request/response envelope, testing pattern, naming. Anything a contributor adding a new module needs to know to fit in.}}

## 3. Per-module doc template

Each per-module doc follows:

```markdown
Status: draft

# <Module>

## Description
One paragraph: what this module does and what it explicitly does not do.

## Files and folders
Tree or list of files in the module.

## Public surface
Public function / class / type signatures with full types.

## Tests
What tests cover this module — concrete inputs and expected outputs.
```
