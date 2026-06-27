# Layout doc content criteria

Criteria for the layout doc — the doc that maps the repository's directory tree so a reader can orient before opening anything.

## 1. Purpose

A reader opens it and understands the shape of the whole repository — what each meaningful directory is for, from the top level down to the modules worth distinguishing — without opening them. It covers source and docs together, not the docs alone.

## 2. Contents

- Go deeper than the top level. Descend into subdirectories that represent distinct concerns a reader needs to tell apart — e.g. list `src/`, then `src/billing/` and `src/auth/` underneath, not just `src/`.
- Do not list every file. Treat a directory as a leaf and annotate it as a whole when its contents are homogeneous or self-evident, or when finer detail belongs in a design or interface doc.
- One line per directory you list: the directory and a one-line description of its role.
- Annotate purpose, not contents — "service layer for the billing domain," not "contains 14 Python files."
- Cover source and docs together; group or order by intent (source, tests, docs, config, build output) when it helps a reader scan.

## 3. Keeping it current

The layout doc describes the repo as it is, so it drifts the moment the structure changes. Update it as part of any change that moves the structure: when you add, move, rename, or remove a directory or a significant file that the doc covers, update it in the same change. A layout doc that lags behind the code misleads every reader that follows.
