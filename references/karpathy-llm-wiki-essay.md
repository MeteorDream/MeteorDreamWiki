---
title: "Karpathy — LLM Wiki (essay)"
category: references
tags:
  - reference
  - llm
  - knowledge-management
  - source
url: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
summary: An informal essay by Andrej Karpathy describing the LLM Wiki pattern — the founding source for this vault's architecture, conventions, and skill set.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
  - "https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.7
lifecycle: stable
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 1.0
  inferred: 0.0
  ambiguous: 0.0
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: related_to
  - target: "[[entities/andrej-karpathy]]"
    type: related_to
---

# Karpathy — LLM Wiki (essay)

The source essay that defines the [[concepts/llm-wiki-pattern]]. This page is the provenance anchor: anything in this vault tagged "from the essay" traces back here.

**URL:** <https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f>

## Type
Informal essay (markdown gist, intended for copy-paste into an LLM agent). Self-described as "an idea file."

## Purpose (in the author's words)

> Its goal is to communicate the high level idea, but your agent will build out the specifics in collaboration with you. ^[extracted]

> This document is intentionally abstract. It describes the idea, not a specific implementation. The exact directory structure, the schema conventions, the page formats, the tooling — all of that will depend on your domain, your preferences, and your LLM of choice. ^[extracted]

This vault is one such instantiation.

## What the essay covers

| Section | What it establishes |
|---|---|
| The core idea | Distinguishes the LLM Wiki from RAG ([[concepts/rag-vs-llm-wiki]]) |
| Architecture | The [[concepts/three-layer-architecture]] (raw / wiki / schema) |
| Operations | The [[skills/wiki-operations-trinity]] (Ingest / Query / Lint) |
| Indexing and logging | The role of [[skills/index-and-log-files]] |
| Optional CLI tools | Recommends [[entities/qmd]] for search at scale |
| Tips and tricks | Web Clipper, image handling, graph view, Marp, Dataview |
| Why this works | LLMs as bookkeepers; bookkeeping is the bottleneck |
| Note | The pattern is intentionally domain-agnostic |

## Direct claims worth preserving verbatim

- "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." ^[extracted]
- "The wiki is a persistent, compounding artifact." ^[extracted]
- "You read it; the LLM writes it." (about the wiki layer) ^[extracted]
- "LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass." ^[extracted]
- "The human's job is to curate sources, direct the analysis, ask good questions, and think about what it all means. The LLM's job is everything else." ^[extracted]

## Local provenance

Originally staged at `_raw/llm/knowledge/karpathy_llm_wiki.md` and promoted into this wiki on 2026-06-06 by `/wiki-ingest` (raw mode). Following the **archive-don't-delete** rule introduced 2026-06-06T13:09Z, the original was archived to `source/llm/knowledge/karpathy_llm_wiki.md` (with archive frontmatter recording storage time, source URL, content hash, and the wiki pages it produced) instead of being deleted. Open that file to see the original essay verbatim plus its provenance metadata.

## See also

- [[entities/andrej-karpathy]]
- [[concepts/llm-wiki-pattern]]
- [[concepts/three-layer-architecture]]
- [[skills/wiki-operations-trinity]]
- [[skills/index-and-log-files]]
