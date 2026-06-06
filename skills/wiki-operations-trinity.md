---
title: Wiki Operations Trinity (Ingest / Query / Lint)
category: skills
tags:
  - workflow
  - knowledge-management
  - wiki
  - skill
summary: The three operational primitives that keep an LLM-maintained wiki working — Ingest (add and integrate sources), Query (answer questions and file the answers back), and Lint (health-check structure and contradictions).
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.7
lifecycle: stable
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: implements
  - target: "[[concepts/persistent-knowledge-graph]]"
    type: uses
---

# Wiki Operations Trinity

The three operations that keep the [[concepts/llm-wiki-pattern]] healthy. Each maps to one or more skills in this vault's `.claude/skills/` directory.

## 1. Ingest

> You drop a new source into the raw collection and tell the LLM to process it. ^[extracted]

What the LLM does:

1. Reads the source
2. Discusses key takeaways with the user (optional, depending on workflow)
3. Writes a summary page in the wiki
4. Updates the index
5. Updates relevant entity and concept pages across the wiki
6. Appends an entry to the log

> A single source might touch 10-15 wiki pages. ^[extracted]

This vault implements ingest as several skills:

| Skill | Purpose |
|---|---|
| `wiki-ingest` | Generic document ingest from `OBSIDIAN_SOURCES_DIR` and `_raw/` |
| `data-ingest` | Catch-all for raw text dumps, ChatGPT exports, transcripts |
| `ingest-url` | Fetch and ingest a single URL |
| `claude-history-ingest` | Mine `~/.claude/projects/` |
| `codex-history-ingest` | Mine `~/.codex/sessions/` |
| `hermes-history-ingest` | Mine `~/.hermes/` |
| `wiki-quick-chat-capture` | 60-second capture from the current session to `_raw/` |

**Two operating modes** (the essay's framing): one-at-a-time with human review, or batch with less supervision. This vault supports both, plus a third — `WIKI_STAGED_WRITES=true` — that routes all writes through `_staging/` for explicit human approval. ^[inferred]

## 2. Query

> You ask questions against the wiki. The LLM searches for relevant pages, reads them, and synthesizes an answer with citations. ^[extracted]

Output forms (from the essay):

- Markdown page
- Comparison table
- Slide deck (Marp)
- Chart (matplotlib)
- Canvas

**Key insight:** good answers should be filed back into the wiki as new pages. ^[extracted]

> A comparison you asked for, an analysis, a connection you discovered — these are valuable and shouldn't disappear into chat history. ^[extracted]

This is what makes exploration compound — not just ingestion. This vault provides:

- `wiki-query` — answer questions with citations, optionally fast-path via index-only
- `wiki-agent` — query a *specific* agent's history (e.g. "what did I do in Codex about X")
- `wiki-context-pack` — token-bounded context slice for downstream tasks
- `wiki-research` — multi-round web search filed back into the vault
- `memory-bridge` — diff knowledge across AI tools

## 3. Lint

> Periodically, ask the LLM to health-check the wiki. ^[extracted]

What lint looks for:

- Contradictions between pages
- Stale claims that newer sources have superseded
- Orphan pages with no inbound links
- Important concepts mentioned but lacking their own page
- Missing cross-references
- Data gaps that could be filled with a web search ^[extracted]

> The LLM is good at suggesting new questions to investigate and new sources to look for. ^[extracted]

This vault splits lint into focused skills:

| Skill | What it surfaces |
|---|---|
| `wiki-lint` | Orphans, broken wikilinks, contradictions, missing frontmatter (also `--consolidate` to act-and-report) |
| `wiki-dedup` | Pages covering the same concept under different names |
| `cross-linker` | Missing wikilinks between obviously-related pages |
| `wiki-synthesize` | Co-occurrence patterns that should become synthesis pages |
| `wiki-status` | Pending sources + health overview + insights mode for graph structure |
| `tag-taxonomy` | Tag drift / inconsistent vocabulary |
| `daily-update` | Cron-friendly daily refresh combining the above |

## Why all three matter

Ingest without Query → you have a knowledge base no one reads.
Ingest without Lint → entropy wins; the graph degrades.
Query without Ingest → you re-derive on every question (this is just RAG; see [[concepts/rag-vs-llm-wiki]]).
Lint without Query → you maintain a graph no one consults.

The trinity is interdependent. Skipping one collapses the pattern back into something cheaper but less compounding. ^[inferred]

## See also

- [[concepts/llm-wiki-pattern]]
- [[concepts/three-layer-architecture]]
- [[skills/index-and-log-files]]
