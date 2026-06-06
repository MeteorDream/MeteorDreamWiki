---
title: Persistent Knowledge Graph
category: concepts
tags:
  - knowledge-management
  - graph
  - obsidian
  - synthesis
summary: A property of LLM-maintained wikis — the resulting collection of interlinked markdown pages forms a graph that gets denser, more cross-referenced, and more synthesis-rich with every ingested source.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.55
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: supporting
provenance:
  extracted: 0.45
  inferred: 0.5
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: derived_from
  - target: "[[entities/obsidian]]"
    type: uses
---

# Persistent Knowledge Graph

The emergent structure that the [[concepts/llm-wiki-pattern]] produces. A directory of markdown files where every page declares wikilinks to others — together they form a graph that **gets denser, not just larger**, as new sources arrive.

## Properties

- **Compounding.** Every ingest adds nodes *and* edges. A new entity page often produces backlinks to existing concepts. ^[inferred]
- **Browsable.** Tools like [[entities/obsidian|Obsidian's]] graph view make the shape directly visible: hubs, clusters, orphans. ^[extracted]
- **Queryable.** Both as plain text (grep / [[entities/qmd]]) and structurally (link traversal, [[skills/index-and-log-files|index file]] catalog).
- **Diff-able.** The wiki is a git repo of markdown files — version history, branching, blame are free. ^[extracted]

## What "persistent" means

Two senses, both important:

1. **Persistent across queries** — unlike RAG, where each session starts cold ([[concepts/rag-vs-llm-wiki]]).
2. **Persistent across time** — the wiki survives chat sessions, agent restarts, even agent migrations. The artifact is the markdown directory, not the conversation. ^[inferred]

The second is where the long-term value lives. Conversations evaporate; the graph remains.

## Hub / orphan dynamics

A healthy wiki has a tail of orphan pages (recently added, not yet woven in) and a body of hub pages (concepts referenced by many others). The lint operation in [[skills/wiki-operations-trinity]] watches this distribution — too many orphans and the graph is incoherent; too few and the wiki has stopped accepting new ideas. ^[inferred]

This vault's `wiki-status --insights` mode (in `.claude/skills/wiki-status/`) computes hub rank, bridge pages, tag-cluster cohesion, and graph delta-since-last-run. Those are direct measurements of the graph's health.

## Why markdown + wikilinks

A graph database would also work, but markdown + `[[wikilinks]]` has compounding ergonomic advantages:

- Plain text — readable without any tool
- Git-native — diffable, branchable, blame-able
- Obsidian-native — instant graph view, live preview, plugin ecosystem
- LLM-native — the file format the LLM was trained to write

Trading "structured queries" for "everything else" turns out to be the right call at the scale of a personal wiki (hundreds to a few thousand pages). ^[inferred]

## See also

- [[concepts/llm-wiki-pattern]]
- [[concepts/three-layer-architecture]]
- [[entities/obsidian]]
- [[skills/index-and-log-files]]
