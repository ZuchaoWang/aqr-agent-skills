# Frontend (React + TypeScript)

Include this doc only if the project has a React frontend. Otherwise delete it and remove the reference from `docs/index.md`.

## 1. Stack

- Language: TypeScript.
- Framework: React, functional components with hooks.
- Build tool: {{Vite / Next.js / other}}.
- Styling: {{CSS Modules / Less / Tailwind / other}}.
- UI kit: {{Ant Design / MUI / none / other}}.
- State management: {{React Context / Redux / Zustand / other}}.
- HTTP client: {{fetch wrapper / axios / TanStack Query / other}}.

## 2. Conventions

- One component per file. Default-exported.
- Co-locate component, styles, and tests in one directory.
- Type every component prop; do not rely on inference for public components.
- Use named exports for utilities and hooks; default export for the component.
- Responsive utilities live in {{`utils/layout.ts`}} and are used for any viewport-dependent sizing.

## 3. Tooling

- Format / lint: {{Prettier + ESLint}}. Run on touched files before commit.
- Type check: `tsc --noEmit`. Run before commit.

## 4. Common pitfalls

- {{Add project-specific pitfalls here as they are discovered.}}
