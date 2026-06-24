# Design content criteria

Criteria for every `design.md` in the blueprint: `architecture/design.md` (system-level) and `implementation/[{{layer}}/]{{module_name}}/design.md` (per-module). The same skeleton applies to all of them; the subject sections below adapt it to what the design describes. Layout (which file goes where) lives in `aqr-project-blueprint`; markdown format lives in the project's `docs/rules/doc_markdown.md`.

A `design.md` carries two things: **criteria of good design** (this doc) and **how to record the decisions behind it** (§1 below, reused by `interface.md`).

## 1. Recording design decisions

Every design doc records the decisions that shaped it, so a future reader never has to choose between blindly accepting or blindly reversing a past choice. Adapt the Architecture Decision Record format (Nygard, *Documenting Architecture Decisions*): one record per significant decision, a few lines each.

A decision record has four fields:

- **Context** — value-neutral description of the forces in play (technical, project, political). What problem forced the choice.
- **Decision** — full sentence, active voice: "We will …". The chosen option, stated directly.
- **Status** — Proposed, Accepted, Superseded (with a pointer to the replacement). Old decisions are kept and marked superseded; the history stays relevant.
- **Consequences** — what follows: positive, negative, and neutral. New constraints, follow-up work, or ADRs this one triggers.

Rules:

- One decision per record. If a section holds several, give each its own record rather than merging.
- Context is value-neutral — do not argue here, that belongs in Decision/Consequences.
- Supersede, do not delete. When the context changes and a decision no longer holds, mark it Superseded and add the new one; the dated history is the point.
- Write each as a short conversation with a future developer: complete sentences. Bullets are for layout, not an excuse for fragments.
- Cross-link the research note (`docs/research/`) that informed it, if one exists. When that note's conclusions are folded in, archive it under `docs/archived/research/`.

## 2. What every design.md must contain

Regardless of subject, a design doc is reviewable without reading code and lets a reader reproduce the design's shape from the text.

1. **Summary** — one paragraph: what this design covers and explicitly what it does not. State scope; ambiguous scope creates drift.
2. **The design** — the responsibility principles in §3, then the subject sections in §4–§7 that apply. Omit any that do not.
3. **Key design decisions** — the decision records from §1, as a numbered list. Each record is a few lines. This is the load-bearing section for "why".
4. **Diagrams** — welcome, but describe them in prose first so the doc reads without rendering.

Constraints: describe boundaries explicitly; a reader should be able to draw the shape from the text alone. Conceptual only — implementation detail lives in code.

## 3. Component and module responsibility

Applies whenever the design decomposes the system into units — system components, modules, or frontend components. These are decomposition principles; the subject sections in §4–§7 specialize them. Omit for a design that does not decompose (e.g. a single visualization with no internal units).

- One clear responsibility per unit. If a unit's name needs "and" to describe it, it has more than one responsibility and is a candidate to split.
- Boundary integrity: a unit owns its state and hides its internals. Callers and parent units depend on the stated contract, not on how it is implemented.
- State ownership: state lives with the unit that needs to mutate it and is lifted to the nearest common ancestor when shared. Do not spread ownership of one piece of state across multiple units.
- Separate what a unit *is* from what it *does*: pure transformation units take input and produce output; coordination units own state and side effects. Mixing both in one unit is a smell — extract one. In a frontend this is the presentational/container split.
- Do not introduce an abstraction for fewer than three call sites. Duplication is cheaper than a premature abstraction that does not fit.

## 4. System architecture (`architecture/design.md`)

Purpose: a reviewer reads it in ~15 minutes and understands the system shape, component boundaries, data flow, and control/workflow flow, without reading code.

Sections:

1. **Components** — one subsection per major component: role, public surface, dependencies, and **boundaries** (what is inside vs. outside its responsibility). Explicit boundaries are required; "it does X" without "and not Y" is incomplete.
2. **Data flow** — how data enters, is transformed, is stored, and leaves. Prose first, then a diagram. Trace one representative request end to end.
3. **Control and workflow flow** — the dynamic shape: request lifecycle, state transitions, concurrency and ordering, async/sync boundaries, failure and retry paths. This is the part most often missing; without it a reader cannot reason about behavior, only structure.
4. **Key design decisions** — §1 records, including the architecture-shaping ones (framework, storage, communication style).

Constraints: target ~2 pages. A reader should be able to draw the architecture diagram from the text.

## 5. Module (`implementation/.../design.md`, non-visualization)

Purpose: a reviewer reads it in ~10 minutes and understands the module's role, internal structure, data, algorithms, and testing approach.

Sections (omit any that do not apply):

1. **Submodules and interactions** — for each submodule, a one-line role and how it interacts with the others. Omit if the module is flat.
2. **Data model** — persistent structures at a conceptual level (entity names, relationships, key fields). Not a full SQL DDL. Omit if the module has no persistent state.
3. **Key algorithms** — brief pseudocode for any non-obvious algorithm. Only the ones a reviewer needs to understand the design. Omit if the module is straightforward.
4. **Testing approach** — how this module should be tested: behaviors to cover, edge cases that matter, what to mock vs. use real. Conceptual, not a list of test cases.
5. **Key design decisions** — §1 records.

Constraints: target ~1 page. Conceptual only — implementation detail lives in code.

## 6. Frontend component responsibility (overlay)

Apply when the design describes a frontend — whether at the system level (`architecture/design.md` for a frontend app) or a single frontend module. It specializes the responsibility principles in §3 for a component tree, adapting the presentational/container distinction (Abramov): split what a component **is** (presentational) from what it **does** (container) for maintainability, reusability, and testability.

Sections:

1. **Component tree and decomposition** — the component hierarchy. A reader should be able to draw the tree from the text. State the unit of decomposition (route, feature, UI element).
2. **Responsibility boundaries** — apply §3. For a frontend the concrete form is presentational (stateless, props in, UI out, reusable/testable) vs. container (stateful, owns data-fetching and logic, passes props down).
3. **State ownership and flow** — apply §3's state-ownership principle. Distinguish local, shared, and server/cache state and where each lives.
4. **Rendering and layout boundaries** — where layout responsibility sits, responsive behavior, viewport-dependent sizing.
5. **When to split a component** — the triggers that justify extraction (a second use site, a prop list growing past readability, a stateful concern leaking into a presentational component).
6. **Prop discipline** — what crosses component boundaries; props are the contract between presentational and container. Avoid prop drilling past one level; if it recurs, that is a state-ownership decision, not a forwarding task.
7. **Interaction and event boundaries** — which component handles which event and where the resulting state change lands.
8. **Testing surface** — what to test at each layer (presentational via props, container via behavior, integration for flows).
9. **Key design decisions** — §1 records.

Constraints: a reader should be able to draw both the component tree and the state-ownership map from the text. Framework mechanics (which hooks, which store) belong in the project's `docs/rules/frontend_js.md`, not here.

## 7. Visualization (overlay)

Apply when the design describes a single chart or visualization component. Adapt marks-and-channels theory (Munzner, *Visualization Analysis and Design*): a visualization encodes data as **marks** (point, line, area) through **channels** (horizontal position, vertical position, color hue, color luminance, size, shape, tilt). Channel choice is governed by two principles:

- **Expressiveness** — a channel must be capable of expressing the data type it encodes. Order channels (luminance, size) express ordinal and quantitative data; categorical channels (hue, shape) express nominal data. Encoding nominal data on a size channel is a violation; so is encoding quantitative magnitude on hue alone.
- **Effectiveness** — when multiple channels could express a data type, use the most effective one. For quantitative data the effectiveness ordering is roughly: position > length > angle > area > luminance > hue. Position is the most precise; reserve it for the variable the reader must read most accurately.

Sections:

1. **Data shape and chart-type mapping** — the dataset's fields, their types (quantitative / ordinal / nominal / temporal), and the chart type chosen. State which fields map to which marks and channels, and why that mapping is expressive.
2. **Visual encoding** — the marks and channels in use, checked against expressiveness and effectiveness. No double-encoding of one variable across two redundant channels without reason. Colorblind-safe palettes; do not rely on hue alone to distinguish categories.
3. **Axes, scales, legends, labels** — scale choices (linear/log/categorical), axis ranges, whether zero-baselines are required for bar/length encodings, legend and label clarity.
4. **Interaction** — hover, filter, zoom, drill, brush; what each reveals and when to omit it. Interaction is a cost — justify each affordance.
5. **Responsiveness and sizing** — how the chart adapts to viewport and container; aspect ratio; density at small sizes.
6. **Accessibility** — text alternative for the chart's takeaway, keyboard operability, sufficient contrast, redundant non-color encoding. A reader who cannot see color or the chart should still get the message.
7. **When a table beats a chart** — if the reader needs to read precise values or compare rows, a table is the better encoding. State the criterion that picked the chart over a table.
8. **Performance for large datasets** — aggregation, sampling, binning; canvas vs. SVG; the rendering strategy and its limit.
9. **Key design decisions** — §1 records, including the encoding choices (which are themselves design decisions).

Constraints: a reader should be able to choose the encoding without seeing the code. If the same `design.md` also describes non-visualization structure, apply §5 or §6 alongside this section and keep them distinct.
