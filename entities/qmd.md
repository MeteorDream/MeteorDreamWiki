---
title: qmd
category: entities
tags:
  - tool
  - search
  - markdown
  - rag
summary: A local search engine for markdown files with hybrid BM25/vector search and LLM re-ranking. Provides both a CLI and an MCP server, and is the recommended search backend for an LLM Wiki once it outgrows index-file lookups.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.45
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: peripheral
provenance:
  extracted: 0.7
  inferred: 0.25
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: uses
  - target: "[[entities/obsidian]]"
    type: related_to
---

# qmd

> A local search engine for markdown files with hybrid BM25/vector search and LLM re-ranking, all on-device. ^[extracted]

Source: [tobi/qmd](https://github.com/tobi/qmd) — referenced in [[references/karpathy-llm-wiki-essay]].

## Role in the LLM Wiki pattern

At small scale (hundreds of pages), the [[skills/index-and-log-files|index.md catalog]] is enough — the LLM reads the index, then drills into specific pages. At larger scale you want proper search. ^[extracted]

`qmd` provides:

- **Hybrid retrieval** — BM25 for lexical matches, vector for semantic similarity
- **On-device LLM re-ranking** — a small model re-orders candidate hits
- **CLI** — agents can shell out (`qmd query …`)
- **MCP server** — agents that speak MCP can use it as a native tool

This vault's skills (`wiki-query`, `wiki-ingest`, `wiki-status`) consult `qmd` when `QMD_WIKI_COLLECTION` and/or `QMD_PAPERS_COLLECTION` are set in `.env`, and fall back to grep otherwise. The current `.env` leaves these unset, so qmd is dormant.

## CLI search modes (from this vault's skill conventions)

`QMD_CLI_SEARCH_MODE` selects the speed/quality tradeoff:

| Mode | Behavior | When to use |
|---|---|---|
| `quality` | hybrid + LLM rerank | default; best relevance |
| `balanced` | hybrid, no rerank | when `quality` is too slow on CPU |
| `fast` | semantic-only `vsearch` | quick exploratory lookups |

## When to install

- After ingesting more than a few hundred pages
- When grep starts producing too many false positives
- If you want semantic discovery (finding "RNN gradient issues" when the page says "vanishing gradients in recurrent nets")

The `wiki-setup` skill includes optional QMD configuration; see `.env.example` (when present) for installation instructions.

## See also

- [[concepts/llm-wiki-pattern]]
- [[entities/obsidian]]
- [[concepts/rag-vs-llm-wiki]] — qmd applies RAG techniques *to the wiki*, which is itself RAG-resistant
