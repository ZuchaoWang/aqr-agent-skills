# Design content criteria

Criteria for design docs. §1 lists the contents to include (universal, plus the UI overlay); §2 gives the design criteria.

## 1. Contents by case

A design doc may combine cases (e.g. a module that is also a frontend); include the sections from each case that applies.

### 1.1 Universal contents (every design doc)

Every design doc — system, layer, or module — covers the same categories; only the depth scales by level. A unit that has children describes those children as black boxes (their interface and how they compose); a leaf module carries the implementation detail. If the unit follows a well-known pattern (B/S, C/S, MVC, MVVM, microservices, event-driven, …), name it and state only where this unit deviates — do not re-explain the pattern, which already implies its components, data flow, and control flow.

- **Summary** — what this design covers and explicitly what it does not.
- **Public interface (this unit's own) and its conceptual implementation** — the upward contract this unit's parent defined for it, restated and shown as implemented. Shape it by unit type:
  - Website / frontend — the URL route map: each route and what it shows.
  - Network service — the endpoints: method, path, request/response fields, error shape.
  - Other module — the signatures of its public API: the functions or methods it exposes.
- **Decomposition and children's interfaces** — if this unit splits into children, name each child module, its role and boundaries, the public interface it must satisfy (the downward, black-box contract this unit imposes on it), and how the children communicate and compose. For a website, the inter-module interface is the B/S API — the request/response contract between browser and server, and between frontend and backend modules.
- **Data model, state, and persistence** — the conceptual structures this unit owns; what state exists, who owns it, and how it persists. Conceptual level, not a full DDL. Omit if stateless.
- **Data flow and control flow** — how data enters, is transformed, is stored, and leaves; the request lifecycle, state transitions, concurrency, caching, failure and retry paths. Add a diagram when the flow is not obvious from prose, and describe it in prose first.
- **Key algorithms** — for each non-trivial part: a well-known algorithm or pattern → just name it; otherwise brief pseudocode; trivial → nothing. A choice among several strategies is a decision (§2.3), not inline prose.
- **Key design and implementation decisions** — decision records, one per entry; format in §2.3.
- **Testing approach** — behaviors and edge cases to cover; what to mock versus use real.

### 1.2 UI / frontend (overlay)

Apply when the design describes a frontend — a whole app or a single module. For a frontend, decomposition is the component tree, and the inter-component interface is carried by state and interactions — not prop wiring, which is an implementation detail.

- **Component tree** — the component hierarchy; a reader can draw it from the text.
- **State list** — which component owns each piece of state; local, shared, server/cache.
- **Interactions** — which component handles which event and where the state change lands.

## 2. Design criteria

Apply §2.1 and §2.3 to every design doc; apply §2.2 when the case is a UI/frontend.

### 2.1 Universal design criteria

- Reviewable without reading code; a reader can reproduce the design's shape from the text.
- Explicit boundaries — every unit states what is inside and outside its responsibility.
- Conceptual only; implementation detail lives in code.
- No code inventories — exhaustive file lists, internal class and function signatures, and test-function names belong in the code, not here. Document the designed contract — the public interface and why each part is exposed — not the derived internal inventory; review the code after execution.
- Be concise. Do not write code when a list of items will do — describe what and why, not how it is implemented.

Decomposition and responsibility (when the design splits into units):

- Keep business logic independent of frameworks, UI, and data access: do not mix business logic into UI components or put database queries in controllers. Separate domain from infrastructure.
- One clear responsibility per unit; if its name needs "and", it is a candidate to split.
- A unit owns its state and hides its internals; callers depend on the contract, not the implementation.
- State lives with the unit that mutates it; lift shared state to the nearest common ancestor.
- Separate what a unit *is* (pure transformation) from what it *does* (owns state and side effects); mixing both is a smell.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.

### 2.2 UI criteria

- Presentational vs. container: presentational components are stateless (props in, UI out, reusable); containers are stateful (own data and logic, pass props down). Mixing both is a smell.
- Locally-shared state lives at the nearest common ancestor of its consumers; drilling it through layers that don't use it signals it is owned too high.
- Broadly-shared or persistent state (auth, theme, session, cached data) lives in a store or context and is read directly by the components that need it — never drilled down through intermediaries as props.
- Extract a component when the same UI is needed a second time (to avoid copy-paste); also split a component that mixes the two roles — pull data fetching and state into a container, and keep the presentational UI pure.
- A component does not define its own absolute position; that is set by its parent's layout. The component only sizes and lays out its own children.

### 2.3 Recording design decisions

Record each significant decision as a short entry with three fields:

- **Context** — the forces that forced the choice.
- **Current decision** — what was chosen, in one line.
- **Alternatives** — options considered (including past decisions since reversed) and why they were not picked.

One decision per entry. When a decision is reversed, update Current decision and move the old one into Alternatives with a one-line reason.
