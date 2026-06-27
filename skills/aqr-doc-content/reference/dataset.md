# Dataset doc content criteria

Criteria for dataset docs — one doc per dataset.

## 1. Purpose

A reviewer reads it and understands what the dataset contains, where it came from, and how to use it correctly, without opening it. Anyone reading should be able to decide whether and how to use the dataset without opening it.

## 2. Sections

1. **Summary** — one paragraph: what the dataset represents, its size and scope.
2. **Source and provenance** — where the data came from and how to reproduce obtaining it: the download URL or script for sourced data, or the transformation/processing script if the dataset was converted or derived from other data. Include license or terms of use.
3. **Schema** — fields, types, units, allowed values. Point at the data dictionary file if one exists; do not reproduce it inline.
4. **Quality notes** — known gaps, anomalies, duplicates, version drift.
