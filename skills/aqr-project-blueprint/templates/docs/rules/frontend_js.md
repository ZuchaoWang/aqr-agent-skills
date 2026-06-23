# Frontend (JS / TS)

Include this doc only if the project has a JavaScript / TypeScript frontend. Otherwise delete it and remove the reference from `docs/index.md`. The rules below are framework-agnostic; record framework-specific choices (React, Vue, Svelte, ...) in §1.

## 1. Stack

- Language: TypeScript, strict mode.
- Framework: {{React / Vue / Svelte / Solid / other}}.
- Build tool: {{Vite / Next.js / Nuxt / other}}.
- Styling: {{CSS Modules / Less / Tailwind / other}}.
- UI kit: {{Ant Design / MUI / none / other}}.
- State management: {{framework primitive / Zustand / Pinia / Redux / other}}.
- HTTP client: {{fetch wrapper / axios / TanStack Query / other}}.

## 2. Rules

- One component per file. Default-exported.
- Co-locate component, styles, and tests in one directory.
- Type every component prop; do not rely on inference for public components.
- Use named exports for utilities and hooks; default export for the component.
- Responsive helpers (e.g. `px2vw`, `px2vh`) live in {{`utils/layout.ts`}} and are used for any viewport-dependent sizing.

## 3. Tooling

- Format / lint: {{Prettier + ESLint}}. Run on touched files before commit.
- Type check: `tsc --noEmit`. Run before commit.

## 4. Tests

- Framework: {{Vitest / Jest / other}}.
- Layout: co-located `*.test.tsx` next to source, or a parallel `tests/` tree. Pick one and stay consistent.
- Component tests: Testing Library. End-to-end: Playwright.
- Run one test: {{command}}.

## 5. Common pitfalls

- {{Add project-specific pitfalls here as they are discovered.}}
