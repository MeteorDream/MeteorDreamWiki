---
title: RAG vs LLM Wiki
category: concepts
tags:
  - rag
  - llm
  - knowledge-management
  - retrieval
  - synthesis
summary: A contrast between Retrieval-Augmented Generation — which re-derives knowledge from raw chunks at every query — and the LLM Wiki pattern, which compiles a persistent, interlinked artifact at ingest time.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.6
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: supporting
provenance:
  extracted: 0.7
  inferred: 0.25
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: contradicts
---

# RAG vs LLM Wiki

Two architectural choices for using LLMs over personal or domain-specific document collections. They differ in **where synthesis happens** and **what compounds**.

## Retrieval-Augmented Generation (RAG)

The default pattern. NotebookLM, ChatGPT file uploads, most "chat with your docs" products work this way. ^[extracted]

1. Documents are uploaded and chunked into an embedding index
2. At query time, the most-similar chunks are retrieved
3. The LLM generates an answer from those chunks plus the question
4. Nothing is written back

**Compounds:** the index (mechanically). The retrieval cost.
**Does not compound:** synthesis. Cross-document insights. Contradictions. Connections discovered during prior queries.

The LLM "rediscovers knowledge from scratch on every question." ^[extracted] A subtle question requiring synthesis across five documents forces the LLM to re-find and re-piece every time.

## LLM Wiki

The [[concepts/llm-wiki-pattern]]. Synthesis is moved *upstream* of the query.

1. Documents are added to a curated raw collection
2. At ingest time, the LLM reads the source and **integrates** it into a persistent wiki — updating entity pages, revising summaries, flagging contradictions
3. At query time, the LLM searches the wiki (a much smaller, denser set of pages) and answers from already-distilled content
4. Good answers can be filed back into the wiki, so explorations also compound ^[extracted]

**Compounds:** synthesis. Cross-references. Contradiction tracking. The whole knowledge graph.
**Does not compound:** raw retrieval recall on long-tail facts (RAG can be better here when the query asks about a literal phrase that survived only in the source).

## When each wins

| Property | RAG | LLM Wiki |
|---|---|---|
| Synthesis depth | low — re-derived per query | high — accumulated at ingest |
| Long-tail recall | high — exact source chunks | medium — depends on what was distilled |
| Latency per query | medium (retrieval + generation) | low (read pages directly) |
| Up-front cost per source | near zero | meaningful (one ingest pass) |
| Knowledge persistence | none | full |
| Best for | one-off question answering | accumulation over weeks/months |

## Hybrid

Real systems often combine both. This vault optionally uses [[entities/qmd]] for hybrid BM25+vector search over the wiki itself — RAG over distilled pages rather than raw documents. The wiki layer is what makes the retrieval signal-rich. ^[inferred]

## See also

- [[concepts/llm-wiki-pattern]]
- [[concepts/persistent-knowledge-graph]]
- [[entities/qmd]]
