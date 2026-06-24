# Design content criteria

Criteria for common design docs. §1 lists the contents to include by case; §2 gives the design criteria. Where the doc lives in the project is a layout choice; this skill covers content only.

## 1. Contents by case

A design doc may combine cases (e.g. a frontend module that is also a visualization); include the sections from each case that applies.

### 1.1 Universal contents (every design doc)

- **Summary** — what this design covers and explicitly what it does not.
- **Key design decisions** — decision records, format in §2.4.
- **Diagrams** — used to show data flow or control flow; describe each in prose first.

### 1.2 System architecture

- **Components** — one per major component: role, public surface, dependencies, boundaries.
- **Data flow** — how data enters, is transformed, is stored, and leaves.
- **Control and workflow flow** — request lifecycle, state transitions, concurrency, failure and retry paths.

### 1.3 Non-visual module

The default for a per-module design; use §1.5 instead when the module is itself a visualization.

- **Submodules and interactions** — one-line role each, how they interact. Omit if flat.
- **Data model** — persistent structures at a conceptual level, not a full DDL. Omit if stateless.
- **Key algorithms** — brief pseudocode for non-obvious ones. Omit if straightforward.
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
- **Visual encoding** — marks and channels in use (see §2.3).
- **Axes, scales, legends, labels** — scale choices, ranges, zero-baselines for length encodings.
- **Interaction** — hover, filter, zoom, drill; what each reveals.
- **Performance** — aggregation, sampling, canvas vs. SVG, for large datasets.

## 2. Design criteria

Apply §2.1 to every design doc; apply §2.2 or §2.3 when the case matches; apply §2.4 to every design doc.

### 2.1 Universal design criteria

- Reviewable without reading code; a reader can reproduce the design's shape from the text.
- Explicit boundaries — every unit states what is inside and outside its responsibility.
- Conceptual only; implementation detail lives in code.

Decomposition and responsibility (when the design splits into units):

- One clear responsibility per unit; if its name needs "and", it is a candidate to split.
- A unit owns its state and hides its internals; callers depend on the contract, not the implementation.
- State lives with the unit that mutates it; lift shared state to the nearest common ancestor.
- Separate what a unit *is* (pure transformation) from what it *does* (owns state and side effects); mixing both is a smell.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.

### 2.2 UI criteria

- Presentational vs. container: presentational components are stateless (props in, UI out, reusable); containers are stateful (own data and logic, pass props down). Mixing both is a smell.
- Lift shared state to the nearest common ancestor that needs it.
- Props are the boundary contract; avoid prop drilling past one level — if it recurs, that is a state-ownership decision.
- Split on a second use site, an unreadable prop list, or a stateful concern leaking into a presentational component.
- Framework mechanics (hooks, stores) belong in the project's frontend style rules, not here.

### 2.3 Visualization criteria

Adapt marks-and-channels theory (Munzner, *Visualization Analysis and Design*): data is encoded as **marks** (point, line, area) through **channels** (position, color, size, shape). Two principles govern channel choice:

- **Expressiveness** — the channel must suit the data type. Position/length/size suit quantitative and ordinal; hue/shape suit nominal. Nominal data on a size channel, or quantitative magnitude on hue alone, are violations.
- **Effectiveness** — prefer the most precise channel. For quantitative data: position > length > angle > area > luminance > hue. Reserve position for the variable read most accurately.

Further:

- No double-encoding one variable across two redundant channels without a reason.
- Use colorblind-safe palettes; do not rely on hue alone.
- A table is the better encoding when the reader needs precise values or row comparison.

### 2.4 Recording design decisions

Record each significant decision so a future reader need not blindly accept or reverse it. Adapt the ADR format (Nygard, *Documenting Architecture Decisions*): one record per decision, a few lines each, with four fields:

- **Context** — value-neutral; the forces that forced the choice.
- **Decision** — active voice: "We will …".
- **Status** — Proposed, Accepted, or Superseded (point to the replacement).
- **Consequences** — what follows: positive, negative, neutral.

- Supersede, do not delete; the dated history is the point.
- Cross-link the research note that informed it; archive the note once conclusions fold in.
