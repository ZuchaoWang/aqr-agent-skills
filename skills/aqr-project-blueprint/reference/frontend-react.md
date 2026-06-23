# Frontend (React + TypeScript) reference

Defaults for a React + TypeScript frontend. Not prescriptive at the framework level — the blueprint recommends Vite + React + TypeScript as the starting point, but the rules below apply regardless of the chosen framework.

## 1. Stack defaults

- Build tool: Vite. Switch to Next.js only when SSR, file-based routing, or i18n is a hard requirement.
- Language: TypeScript, strict mode.
- Styling: CSS Modules (`.module.css`) for new projects. Less Modules (`.module.less`) is acceptable for projects that already use Less. Tailwind is acceptable when the team has chosen it deliberately.
- UI kit: Ant Design (Chinese locale by default if the project's primary audience is Chinese users) or MUI. None is fine for design-system-heavy products.
- State: React Context for app-wide singletons (current user, theme, locale); local `useState` for component-local state; Zustand or Jotai for cross-component state that does not deserve a context.
- HTTP: a thin `fetch` wrapper for simple apps; TanStack Query for cache-heavy apps.
- Routing: React Router (`react-router-dom`) for SPAs; the framework's built-in router for Next.js.

## 2. Component rules

- One component per directory. Directory name = component name in kebab-case.
- Default-exported component. Named exports for utilities and hooks.
- Co-locate: `index.tsx` (component), `style.module.less` (styles), `test.tsx` (tests).
- Type every prop explicitly. No `any` on public component props.
- Hooks prefixed with `use`. One hook per file when the hook is non-trivial.

## 3. Source layout

A typical Vite + React frontend:

```
frontend/src/
  main.tsx            # entry; router setup
  pages/              # one directory per route
  components/         # reusable UI components
  context/            # React context providers
  hooks/              # custom hooks
  api/                # backend client (one file per resource)
  utils/              # pure helpers
  styles/             # global styles and mixins
  assets/             # images, fonts
```

The frontend doc under `docs/implementation/frontend/` mirrors this structure: one `.md` per top-level directory.

## 4. Responsive sizing

For dashboards or fixed-viewport apps (large-screen displays, kiosks), do not rely on CSS media queries alone. Use viewport-relative helpers (`px2vw`, `px2vh`, `px2font`) so the layout scales with the target viewport. For consumer web with diverse viewports, use Tailwind breakpoints or CSS container queries instead.

State the chosen approach in `docs/rules/frontend-react.md`.

## 5. Tooling

- Format / lint: Prettier + ESLint (flat config).
- Type check: `tsc --noEmit`.
- Test: Vitest + Testing Library for component tests; Playwright for end-to-end.

Run on touched files only.

## 6. Common pitfalls

- Inline `style={{...}}` objects that recreate on every render. Hoist them or use CSS Modules.
- Effects that should be event handlers. Use effects only for synchronization with external systems.
- Unmemoized values in dependency arrays. Use `useMemo` / `useCallback` only when measurements show they help — premature memoization adds noise.
- Throttled or debounced callbacks whose trailing edge fires after unmount. Always cancel in cleanup.
- Direct DOM manipulation bypassing React. Use refs and effects; do not query the DOM from inside a component.
