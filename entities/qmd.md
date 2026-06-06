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
  - "https://github.com/Ar9av/obsidian-wiki/blob/main/README.md"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T13:29:22Z
base_confidence: 0.55
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

## Setup (per the framework's README)

```bash
# 1. Install QMD. For MCP mode, also add it to your MCP config.
# 2. Index your wiki and/or source documents:
qmd index --name wiki   /path/to/your/vault
qmd index --name papers /path/to/your/sources

# 3. Set the collection names + transport in .env:
#    QMD_WIKI_COLLECTION=wiki        # used by wiki-query
#    QMD_PAPERS_COLLECTION=papers    # used by wiki-ingest (source discovery)
#    QMD_TRANSPORT=mcp               # mcp | cli
#    QMD_CLI_SEARCH_MODE=quality     # quality | balanced | fast
```

`QMD_TRANSPORT=mcp` uses an agent-configured MCP server. `QMD_TRANSPORT=cli` runs the local `qmd` command directly. ^[extracted]

## What changes with QMD enabled

- **`wiki-query`** runs a semantic pass (lex+vec) against your wiki collection before falling back to Grep. Finds conceptually related pages even when the exact terms don't match. ^[extracted]
- **`wiki-ingest`** queries your papers collection before writing a new page — surfaces related sources, spots contradictions, and decides whether to create a new page or merge into an existing one. ^[extracted]

Both skills degrade gracefully: if `QMD_WIKI_COLLECTION` / `QMD_PAPERS_COLLECTION` are not set, they skip the QMD step silently and use Grep instead. ^[extracted]

## Status in MeteorDreamWiki

The current `.env` leaves QMD unset — qmd is dormant. With only ~16 pages in the vault, the index file is plenty and grep handles the rest. Worth revisiting around the 200–500 page mark.

## See also

- [[concepts/llm-wiki-pattern]]
- [[entities/obsidian]]
- [[entities/obsidian-wiki-framework]]
- [[concepts/rag-vs-llm-wiki]] — qmd applies RAG techniques *to the wiki*, which is itself RAG-resistant
