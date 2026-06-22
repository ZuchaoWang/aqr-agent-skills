# Project layout

Top-level layout of this repository. One-line annotation per directory and root file. Update this doc whenever the top-level shape changes.

```
{{root tree here, e.g.:

backend/                # Python service
  src/
  tests/
  scripts/
frontend/               # React + TypeScript client
  src/
  public/
docs/                   # Documentation — start with docs/index.md
  project/              # Mission, scope, roadmap
  architecture/         # System-level design
  implementation/       # Per-module implementation reference
  research/             # Background research
  data/                 # Dataset docs
  conventions/          # Code, doc, and test conventions
issues/                 # Issue records (one directory per issue)
tmp/                    # Scratch directory
}}
```

## Conventions

- `docs/` is spec-driven: code is written to satisfy documented specifications, not the other way around.
- `issues/` holds issue records in the `aqr-issue-records` format. One directory per issue, grouped by `YYYYMM`.
- `tmp/` is local scratch; nothing in `tmp/` is committed.
