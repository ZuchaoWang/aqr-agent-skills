# Design content criteria

Criteria for every `design.md` in the blueprint: `architecture/design.md` (system-level) and `implementation/[{{layer}}/]{{module_name}}/design.md` (per-module). Layout (which file goes where) lives in `aqr-project-blueprint`; markdown format lives in the project's `docs/rules/doc_markdown.md`.

A design.md is governed by two things: **what it contains** (§1, by case) and **how well it is designed and recorded** (§2, criteria). When writing or reviewing one, find the case in §1 for the section list, then apply the universal criteria in §2.1, the subject criteria in §2.2 or §2.3 if they apply, and record decisions per §2.4.

## 1. Contents by case

What sections a design.md contains depends on what it describes. The four cases below are not mutually exclusive — a single design.md may combine them (e.g. a frontend module that is also a visualization); include the sections from each case that applies.

### 1.1 Universal contents (every design.md)

- **Summary** — one paragraph: what this design covers and explicitly what it does not. State scope; ambiguous scope creates drift.
- **Key design decisions** — the decision records, format in §2.4. The load-bearing section for "why".
- **Diagrams** — welcome, but describe them in prose first so the doc reads without rendering.

### 1.2 System architecture (`architecture/design.md`)

A reviewer reads it in ~15 minutes and understands the system shape, component boundaries, data flow, and control/workflow flow, without reading code.

- **Components** — one subsection per major component: role, public surface, dependencies, and boundaries (inside vs. outside its responsibility).
- **Data flow** — how data enters, is transformed, is stored, and leaves. Trace one representative request end to end.
- **Control and workflow flow** — the dynamic shape: request lifecycle, state transitions, concurrency and ordering, async/sync boundaries, failure and retry paths. The part most often missing; without it a reader can reason about structure, not behavior.

### 1.3 Non-visual module (`implementation/.../design.md`)

A reviewer reads it in ~10 minutes and understands the module's role, internal structure, data, algorithms, and testing approach. This is the default case for `implementation/.../design.md`; use §1.5 instead when the module is itself a visualization.

- **Submodules and interactions** — for each submodule, a one-line role and how it interacts with the others. Omit if the module is flat.
- **Data model** — persistent structures at a conceptual level (entity names, relationships, key fields). Not a full SQL DDL. Omit if the module has no persistent state.
- **Key algorithms** — brief pseudocode for any non-obvious algorithm a reviewer needs to understand the design. Omit if the module is straightforward.
- **Testing approach** — how this module should be tested: behaviors to cover, edge cases that matter, what to mock vs. use real. Conceptual, not a list of test cases.

### 1.4 UI / frontend (overlay)

Apply when the design describes a frontend — at the system level (`architecture/design.md` for a frontend app) or a single frontend module.

- **Component tree and decomposition** — the component hierarchy; a reader should be able to draw the tree from the text. State the unit of decomposition (route, feature, UI element).
- **Responsibility boundaries** — for each significant component, whether it is presentational or container (see §2.2).
- **State ownership and flow** — which component owns each piece of state and why; local, shared, and server/cache state and where each lives.
- **Rendering and layout boundaries** — where layout responsibility sits, responsive behavior, viewport-dependent sizing.
- **When to split a component** — the triggers that justify extraction.
- **Prop discipline** — what crosses component boundaries; props are the contract between presentational and container.
- **Interaction and event boundaries** — which component handles which event and where the resulting state change lands.
- **Testing surface** — what to test at each layer (presentational via props, container via behavior, integration for flows).

### 1.5 Visualization (overlay)

Apply when the design describes a single chart or visualization component. Use this case instead of §1.3 when an `implementation/.../design.md` is itself a visualization.

- **Data shape and chart-type mapping** — the dataset's fields, their types (quantitative / ordinal / nominal / temporal), and the chart type chosen; which fields map to which marks and channels.
- **Visual encoding** — the marks and channels in use (see §2.3).
- **Axes, scales, legends, labels** — scale choices (linear/log/categorical), axis ranges, zero-baselines for length encodings, legend and label clarity.
- **Interaction** — hover, filter, zoom, drill, brush; what each reveals.
- **Responsiveness and sizing** — how the chart adapts to viewport and container; aspect ratio; density at small sizes.
- **Accessibility** — text alternative for the chart's takeaway, keyboard operability, sufficient contrast, redundant non-color encoding.
- **When a table beats a chart** — the criterion that picked the chart over a table.
- **Performance for large datasets** — aggregation, sampling, binning; canvas vs. SVG; the rendering strategy and its limit.

## 2. Design criteria

The quality rules below are not all one kind: §2.1 is universal, §2.2–§2.3 are subject-specific, §2.4 is a recording practice. Apply §2.1 to every design.md; apply §2.2 or §2.3 only when the case matches; apply §2.4 to every design.md.

### 2.1 Universal design criteria

Apply to every design.md.

- Reviewable without reading code: a reader can reproduce the design's shape from the text alone.
- Explicit boundaries: every unit states what is inside and outside its responsibility; "it does X" without "and not Y" is incomplete.
- Conceptual only — implementation detail lives in code.

Decomposition and responsibility (when the design splits into units — components, modules, submodules):

- One clear responsibility per unit. If a unit's name needs "and" to describe it, it has more than one responsibility and is a candidate to split.
- Boundary integrity: a unit owns its state and hides its internals. Callers and parent units depend on the stated contract, not on how it is implemented.
- State ownership: state lives with the unit that needs to mutate it and is lifted to the nearest common ancestor when shared. Do not spread ownership of one piece of state across multiple units.
- Separate what a unit *is* from what it *does*: pure transformation units take input and produce output; coordination units own state and side effects. Mixing both in one unit is a smell — extract one. In a frontend this is the presentational/container split (§2.2).
- Do not introduce an abstraction for fewer than three call sites. Duplication is cheaper than a premature abstraction that does not fit.

### 2.2 UI criteria

Specialize §2.1 for a component tree.

- Presentational vs. container (the concrete form of "is vs. does"): presentational components are stateless, take props in, render UI out, and are reusable and testable; container components are stateful, own data-fetching and logic, and pass props down. Mixing both in one component is a smell worth recording as a decision.
- State ownership: distinguish local, shared, and server/cache state. Lift shared state to the nearest common ancestor that needs it; the owner should be the component that mutates it.
- Prop discipline: props are the boundary contract. Avoid prop drilling past one level — if it recurs, that is a state-ownership decision, not a forwarding task.
- When to split: a second use site, a prop list growing past readability, or a stateful concern leaking into a presentational component.
- Framework mechanics (which hooks, which store) belong in the project's `docs/rules/frontend_js.md`, not in the design.

### 2.3 Visualization criteria

Adapt marks-and-channels theory (Munzner, *Visualization Analysis and Design*): a visualization encodes data as **marks** (point, line, area) through **channels** (horizontal position, vertical position, color hue, color luminance, size, shape, tilt). Channel choice is governed by two principles:

- **Expressiveness** — a channel must be capable of expressing the data type it encodes. Order channels (luminance, size) express ordinal and quantitative data; categorical channels (hue, shape) express nominal data. Encoding nominal data on a size channel is a violation; so is encoding quantitative magnitude on hue alone.
- **Effectiveness** — when multiple channels could express a data type, use the most effective one. For quantitative data the ordering is roughly: position > length > angle > area > luminance > hue. Position is the most precise; reserve it for the variable the reader must read most accurately.

Further rules:

- No double-encoding of one variable across two redundant channels without a reason.
- Colorblind-safe palettes; do not rely on hue alone to distinguish categories.
- A reader should be able to choose the encoding without seeing the code.
- When the reader needs precise values or row comparison, a table is the better encoding — state the criterion that picked the chart over a table.

### 2.4 Recording design decisions

Every design doc records the decisions that shaped it, so a future reader never has to choose between blindly accepting or blindly reversing a past choice. Adapt the Architecture Decision Record format (Nygard, *Documenting Architecture Decisions*): one record per significant decision, a few lines each.

A decision record has four fields:

- **Context** — value-neutral description of the forces in play (technical, project, political). What problem forced the choice.
- **Decision** — full sentence, active voice: "We will …". The chosen option, stated directly.
- **Status** — Proposed, Accepted, Superseded (with a pointer to the replacement). Old decisions are kept and marked superseded; the history stays relevant.
- **Consequences** — what follows: positive, negative, and neutral. New constraints, follow-up work, or ADRs this one triggers.

Rules:

- One decision per record. If a section holds several, give each its own record rather than merging.
- Context is value-neutral — do not argue here; that belongs in Decision/Consequences.
- Supersede, do not delete. When the context changes and a decision no longer holds, mark it Superseded and add the new one; the dated history is the point.
- Write each as a short conversation with a future developer: complete sentences. Bullets are for layout, not an excuse for fragments.
- Cross-link the research note (`docs/research/`) that informed it, if one exists. When that note's conclusions are folded in, archive it under `docs/archived/research/`.
