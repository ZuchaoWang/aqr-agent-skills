# Research doc content criteria

Criteria for `docs/research/`: `background.md`, `related_works.md`, `brainstorm.md`. Research docs inform decisions; they are not specs. Once a research note's conclusions are folded into `architecture/` or `implementation/`, move the note under `docs/archived/research/` and have the new doc reference it. Layout lives in `aqr-project-blueprint`; markdown format lives in the project's `docs/rules/doc_markdown.md`.

## 1. `background.md`

Purpose: a reviewer reads it and understands the domain knowledge and motivations behind the project's technical choices — everything that is **not** an actively-used concept.

`background.md` is the complement of the project's active concepts (`project.md` §5): the active concepts live there; `background.md` holds everything else, including concepts that were considered but are **not** used. Do not duplicate the active-concept set here.

Sections:

1. **Summary** — one paragraph: what this background doc covers and why it matters to the project.
2. **Concepts and context** — one subsection per topic: the knowledge, motivation, or concept, its relevance to the project, and pointers to external sources. Include concepts considered and rejected, with a one-line reason they were not adopted — the rejected set is as useful as the adopted one.
3. **Implications for design** — how this background constrains or informs the system design.

Constraints: target ~2 pages. Write for a technically literate reader who is unfamiliar with the domain. Avoid re-stating what the code already makes obvious. When a concept graduates to active use, move its definition to the active-concepts section and leave only the background/motivation here.

## 2. `related_works.md` (minimal)

De-prioritized. Purpose: a reviewer reads it and understands what existing work is related, how it differs, and what the project borrows or avoids.

Sections:

1. **Summary** — one paragraph: the landscape of related work and what gap this project fills.
2. **Entries** — one subsection per related work: name, brief description, how it relates to this project (similar, complementary, or superseded), and what if anything was borrowed.

Constraints: each entry must state the relationship explicitly; "this also does X" is not enough. Target ~1–2 pages. Keep only when the comparison genuinely informs a decision; otherwise omit the file.

## 3. `brainstorm.md` (minimal)

De-prioritized. Purpose: a reviewer reads it and understands what design options were considered, what trade-offs were discussed, and what was decided and why. The goal is to capture reasoning so it does not have to be reconstructed later.

Sections:

1. **Summary** — one paragraph: the design question being explored.
2. **Options** — one subsection per option considered: description, advantages, disadvantages.
3. **Decision** — which option was chosen, or that the question is still open. If decided, cross-link to the `design.md` decision record that adopted it.

Constraints: target ~1–2 pages. If the decision is still open, mark the open question explicitly. Once decided, the reasoning may be folded into a `design.md` decision record and this note archived.
