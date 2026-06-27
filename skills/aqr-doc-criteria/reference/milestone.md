# Milestone doc content criteria

Criteria for a milestone doc — one file per short-term development goal. It is the stable contract between the human and the AI for one development objective.

## 1. Purpose

A human reads it and knows exactly what change is wanted and what "done" means before implementation begins. A milestone describes **what should change**, not what currently exists — the current state lives in the architecture and implementation docs. It is the primary artifact a human reviews before AI implementation, and the reference the AI works against throughout.

## 2. Sections

1. **Goal** — one paragraph: the desired change to the system and the user intent behind it. State what should be different afterward, not how the system works today.
2. **Decisions** — agreed decisions that scope the work, each with a one-line rationale and date. Record what was decided; unresolved questions may sit here while open and are removed once settled.
3. **Acceptance criteria** — the observable conditions that must hold for the milestone to be complete, each stated so a reviewer can check it without reading code.

## 3. Constraints

- A milestone is a mutable working document. Revise it as intent is clarified; do not freeze it at the start.
- Keep transient artifacts out. Planning, task decomposition, and execution progress are not recorded here — they are ephemeral. The milestone holds stable intent and acceptance criteria, not a work log.
- When implementation is complete, fold the durable outcome into the architecture, interface, or implementation docs, then freeze the milestone. Create a new milestone for the next objective.
