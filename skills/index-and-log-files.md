---
title: Index and Log Files
category: skills
tags:
  - workflow
  - wiki
  - schema
  - skill
summary: Two special files at the vault root that anchor an LLM-maintained wiki — index.md is a content catalog the LLM reads first when answering queries; log.md is a chronological append-only record of operations.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.6
lifecycle: stable
lifecycle_changed: 2026-06-06
tier: supporting
provenance:
  extracted: 0.8
  inferred: 0.15
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: implements
  - target: "[[skills/wiki-operations-trinity]]"
    type: uses
---

# Index and Log Files

Two special files at the vault root that anchor the [[concepts/llm-wiki-pattern]]. They serve different purposes and should not be conflated.

## index.md — content-oriented

A catalog of everything in the wiki: each page listed with a link, a one-line summary, optionally metadata (date, source count). Organized by category (entities, concepts, references, etc.). ^[extracted]

- Updated on every ingest ^[extracted]
- Read first by the LLM when answering a query, then drilled into specific pages ^[extracted]
- "Works surprisingly well at moderate scale (~100 sources, ~hundreds of pages) and avoids the need for embedding-based RAG infrastructure" ^[extracted]

In this vault, `index.md` is auto-maintained by every ingest skill and refreshed end-to-end by `daily-update`.

### Why an index file at all?

Because LLMs have finite context. Reading every page in the vault to answer one question is wasteful at any scale beyond ~20 pages. The index is the **cheap retrieval path** — it costs ~one read to identify the relevant pages, then full-page reads for only those. This vault's `wiki-query` skill exposes it as a "fast mode" that skips full-page reads entirely. ^[inferred]

## log.md — chronological

Append-only record of what happened and when — ingests, queries, lint passes. ^[extracted]

A useful tip from the essay:

> If each entry starts with a consistent prefix (e.g. `## [2026-04-02] ingest | Article Title`), the log becomes parseable with simple unix tools — `grep "^## \[" log.md | tail -5` gives you the last 5 entries. ^[extracted]

This vault uses a slightly different convention (single-line entries with bracketed ISO timestamps and a verb prefix like `INGEST`, `LINT`, `STATUS_INSIGHTS`), but the principle is the same: **machine-greppable, human-readable**.

## hot.md — semantic snapshot (this vault's addition)

Karpathy's essay describes only `index.md` and `log.md`, but this vault adds a third special file: `hot.md`. ^[inferred]

`hot.md` is a ~500-word semantic snapshot of:

- **Recent Activity** — the last 3 operations in conceptual terms
- **Active Threads** — ongoing topics the user is exploring
- **Key Takeaways** — the highest-signal claims from recent ingests
- **Flagged Contradictions** — known unresolved tensions in the wiki

Where `index.md` is *content* and `log.md` is *chronology*, `hot.md` is *attention* — what's hot in the user's current work. It's read by skills that need to orient quickly without scanning the full graph. ^[inferred]

## .manifest.json — bookkeeping

A fourth file (machine-only, not in the essay): `.manifest.json` at the vault root tracks every source ever ingested — path, hash, mtime, source type, project, pages created/updated. ^[inferred]

This is what enables append-mode ingest. Without it, every run would have to rediscover what's new.

## See also

- [[concepts/llm-wiki-pattern]]
- [[concepts/three-layer-architecture]]
- [[skills/wiki-operations-trinity]]
