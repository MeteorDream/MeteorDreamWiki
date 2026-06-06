---
title: LLM Wiki Pattern
category: concepts
tags:
  - knowledge-management
  - llm
  - wiki
  - obsidian
  - pattern
summary: A pattern where an LLM incrementally builds and maintains a persistent, interlinked markdown wiki between the user and raw sources — knowledge is compiled once, then kept current, instead of re-derived on every query.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.65
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[concepts/three-layer-architecture]]"
    type: uses
  - target: "[[concepts/rag-vs-llm-wiki]]"
    type: contradicts
  - target: "[[references/memex]]"
    type: derived_from
  - target: "[[entities/andrej-karpathy]]"
    type: related_to
---

# LLM Wiki Pattern

A workflow pattern in which an LLM agent **incrementally builds and maintains a persistent wiki** of structured, interlinked markdown files — placed between the user and their raw sources. Whenever a new source arrives, the LLM reads it, extracts what's useful, and *integrates* it into existing pages: updating entities, revising summaries, flagging contradictions, strengthening synthesis.

The defining property: **the wiki is a persistent, compounding artifact.** Cross-references already exist. Contradictions have already been flagged. Synthesis already reflects everything ingested.

## Why it exists

Most LLM-document workflows look like [[concepts/rag-vs-llm-wiki|RAG]]: chunks are retrieved at query time and the LLM rediscovers knowledge on every question. Nothing accumulates. A subtle question requiring synthesis across five documents forces the LLM to re-find and re-piece each time.

The LLM Wiki pattern moves the synthesis *upstream* of the query. The compounding work happens at ingest, not retrieval.

## Division of labor

The user owns:
- **Sourcing** — choosing what to feed in
- **Exploration** — asking good questions
- **Direction** — what to emphasize and what to ignore

The LLM owns:
- Reading
- Cross-referencing
- Filing
- Bookkeeping (timestamp updates, link maintenance, frontmatter)

Karpathy's working setup: LLM agent on one side, [[entities/obsidian|Obsidian]] on the other. The LLM edits; the human browses the graph in real time. ^[extracted]

> Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase. ^[extracted]

## Use cases

The pattern generalizes to any domain where knowledge accumulates over time:

- **Personal** — goals, health, journal entries, podcast notes
- **Research** — multi-month deep dives across papers, articles, reports
- **Reading a book** — building a fan-wiki-style companion as you go (the essay cites Tolkien Gateway as the inspiration form)
- **Business / team** — internal wikis fed by Slack, meeting transcripts, customer calls
- **Competitive analysis, due diligence, trip planning, hobby deep-dives**

## Why it works

The bottleneck in human-maintained wikis is not reading or thinking — it's bookkeeping. Cross-reference updates, summary refreshes, contradiction tracking, consistency across dozens of pages. Humans abandon wikis because maintenance cost grows faster than value. ^[extracted]

LLMs don't get bored, don't forget cross-references, and can touch 15 files in one pass. The wiki stays maintained because **the cost of maintenance is near zero.** ^[extracted]

This wiki is itself an instance of the pattern.

## See also

- [[concepts/three-layer-architecture]] — raw sources / wiki / schema
- [[concepts/rag-vs-llm-wiki]] — the contrast that motivates the pattern
- [[concepts/persistent-knowledge-graph]] — the graph property the pattern produces
- [[skills/wiki-operations-trinity]] — Ingest / Query / Lint as the operational primitives
- [[references/karpathy-llm-wiki-essay]] — the source essay
- [[entities/andrej-karpathy]] — author of the pattern
- [[references/memex]] — Vannevar Bush's 1945 vision, the spiritual predecessor
