# Node project reference

Defaults for a Node-based frontend or service following the blueprint. Adjust per project; record deviations in `docs/rules/frontend_js.md` (frontend) or a project-specific backend doc.

## 1. Versions

- Node 24 LTS by default for new projects. Pin in `.nvmrc` (single line, no `v` prefix).
- The system `node` shipped by Linux distributions is often far behind. Document the workaround in `CLAUDE.md`: either `nvm use` from the project root, or invoke Node via an absolute path to the version manager's installed binary.

## 2. Package manager

Pick one per project and commit the choice:

- `npm` — default; ships with Node. Lock file: `package-lock.json`.
- `pnpm` — fast, disk-efficient. Lock file: `pnpm-lock.yaml`. Set `packageManager` in `package.json`.
- `yarn` — established alternative. Lock file: `yarn.lock`. Set `packageManager` in `package.json`.

For monorepos, `pnpm` is the default recommendation due to workspace handling.

## 3. Frameworks

### 3.1 Frontend (React)

- Build tool: Vite (default) or Next.js (if SSR / routing / i18n is required).
- Framework: React, functional components with hooks.
- TypeScript: yes. Strict mode.
- Styling: CSS Modules, Less modules, or Tailwind. Pick one per project.
- UI kit: Ant Design, MUI, or none. Pick one per project.
- State: React Context for simple cases; Zustand, Jotai, or Redux Toolkit for complex state.
- HTTP: a thin `fetch` wrapper, or TanStack Query for cache-heavy apps.

### 3.2 Backend (Node)

For Node backends, the blueprint does not take a strong position. Pick Express, Fastify, Hono, or NestJS per project. Document the choice in `docs/rules/backend-<framework>.md`.

## 4. Lint / format / type-check

- Format: Prettier.
- Lint: ESLint (flat config, ESLint 9+).
- Type check: `tsc --noEmit`.

Run on touched files only:

```bash
npx prettier --write path/to/file.tsx
npx eslint --fix path/to/file.tsx
npx tsc --noEmit
```

Editor: configure format-on-save with Prettier; configure ESLint to fix on save.

## 5. Tests

- Framework: Vitest (default) or Jest.
- Layout: co-located `*.test.tsx` next to source, or a parallel `tests/` tree. Pick one rule per project.
- For component tests: Testing Library.
- For end-to-end: Playwright.

## 6. Monorepo

If the project has more than one deployable (e.g. frontend + backend in one repo), prefer:

```
frontend/    # React + Vite
backend/     # Python or Node service
docs/        # shared docs
```

With per-tree `package.json` / `pyproject.toml`. Avoid npm workspaces or pnpm workspaces unless the project has shared JS packages — they add complexity without proportional value when the trees are independent.

## 7. Common pitfalls

- System Node being the wrong version. Pin in `.nvmrc` and document in `CLAUDE.md`.
- Mixing CommonJS and ESM in one project. Pick one (`"type": "module"` in `package.json` for new projects).
- Not committing the lockfile. Always commit.
- Letting `node_modules/` into the working tree state. The shipped `.gitignore` covers this; do not remove the entry.
