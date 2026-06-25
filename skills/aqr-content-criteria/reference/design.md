# Design content criteria

Criteria for common design docs. §1 lists the contents to include by case; §2 gives the design criteria. Where the doc lives in the project is a layout choice; this skill covers content only.

## 1. Contents by case

A design doc may combine cases (e.g. a frontend module that is also a visualization); include the sections from each case that applies.

### 1.1 Universal contents (every design doc)

- **Summary** — what this design covers and explicitly what it does not.
- **Key design decisions** — decision records, format in §2.4.
- **Diagrams** — used to show data flow or control flow; describe each in prose first.

### 1.2 System architecture

Design at the system layer: treat each module as a black box, define only its public interface, and put the effort into how the modules work together. Do not open a module's internals here — that belongs in the module's own design.

If the system follows a well-known pattern (B/S, C/S, MVC, MVVM, microservices, event-driven, …), just name it and state the key interfaces between its parts. Do not re-explain the pattern; it already implies its components, data flow, and control flow. Otherwise, describe the system directly:

- **Modules** — one per major module: role, public interface, dependencies, boundaries.
- **Data flow** — how data enters, is transformed, is stored, and leaves.
- **Control and workflow flow** — request lifecycle, state transitions, concurrency, failure and retry paths.

Even when a pattern is named, add a bullet only where this system deviates from or extends it.

### 1.3 Non-visual module

The default for a per-module design; use §1.5 instead when the module is itself a visualization. Design at this module's layer: if it has submodules, treat each as a black box, define only its public interface, and focus on how they compose. Give implementation detail only for a leaf module — one with no further submodules to delegate to — and even then, not all detail. The same layering rule applies recursively.

For a leaf module:

- **Data model** — persistent structures at a conceptual level, not a full DDL. Omit if stateless.
- **Algorithms and implementation** — for each non-trivial part, choose by how familiar it is:
  - A well-known algorithm or design pattern → just name it.
  - Other non-trivial logic → brief pseudocode.
  - Trivial → write nothing.
- If a part requires choosing among several strategies, record it as a decision (§2.4), not as prose.
- For a non-trivial single-path implementation, leave an implementation hint — one or two lines on the intended approach.
- **Testing approach** — behaviors and edge cases to cover, what to mock vs. use real.

### 1.4 UI / frontend (overlay)

Apply when the design describes a frontend — a whole app or a single module.

- **Component tree** — the component hierarchy; a reader can draw it from the text.
- **Prop list** — what crosses each component boundary.
- **State list** — which component owns each piece of state; local, shared, server/cache.
- **Interactions** — which component handles which event and where the state change lands.

### 1.5 Visualization (overlay)

Apply when the design describes a single chart or visualization component.

- **Data shape and chart type** — field types (quantitative / ordinal / nominal / temporal) and the chart chosen; which fields map to which marks and channels.
- **Visual encoding** — explicitly state how each visual property is decided: x, y, z (z-index), color, size, width, and the rest. For each, name the data field (or constant) it encodes and why (see §2.3).
- **Axes, scales, legends, labels** — scale choices, ranges, zero-baselines for length encodings.
- **Interaction** — hover, filter, zoom, drill; what each reveals.
- **Performance** — aggregation, sampling, canvas vs. SVG, for large datasets.

## 2. Design criteria

Apply §2.1 to every design doc; apply §2.2 or §2.3 when the case matches; apply §2.4 to every design doc.

### 2.1 Universal design criteria

- Design one layer at a time. Treat units at the next layer as black boxes: define their public interface and design how they compose; do not open their internals in this doc. Give implementation detail only for a leaf unit with no further sub-units, and not all of it.
- Reviewable without reading code; a reader can reproduce the design's shape from the text.
- Explicit boundaries — every unit states what is inside and outside its responsibility.
- Conceptual only; implementation detail lives in code.
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
- Extract a component when the same UI is needed a second time (to avoid copy-paste); also split on an unreadable prop list or a stateful concern leaking into a presentational component.
- A component does not define its own absolute position; that is set by its parent's layout. The component only sizes and lays out its own children.

### 2.3 Visualization criteria

Adapt marks-and-channels theory (Munzner, *Visualization Analysis and Design*): data is encoded as **marks** (point, line, area) through **channels** (position, color, size, shape). Two principles govern channel choice:

- **Expressiveness** — the channel must suit the data type. Position/length/size suit quantitative and ordinal; hue/shape suit nominal. Nominal data on a size channel, or quantitative magnitude on hue alone, are violations.
- **Effectiveness** — prefer the most precise channel. For quantitative data: position > length > angle > area > luminance > hue. Reserve position for the variable read most accurately.

Further:

- No double-encoding one variable across two redundant channels without a reason.
- A table is the better encoding when the reader needs precise values or row comparison.

### 2.4 Recording design decisions

Record each significant decision as a short entry with three fields:

- **Context** — the forces that forced the choice.
- **Current decision** — what was chosen, in one line.
- **Alternatives** — options considered (including past decisions since reversed) and why they were not picked.

One decision per entry. When a decision is reversed, update Current decision and move the old one into Alternatives with a one-line reason.
