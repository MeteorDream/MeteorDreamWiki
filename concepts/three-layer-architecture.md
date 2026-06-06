---
title: Three-Layer Architecture
category: concepts
tags:
  - architecture
  - knowledge-management
  - wiki
  - schema
summary: The structural decomposition of an LLM-maintained knowledge base into three layers — immutable raw sources, an LLM-owned wiki of distilled markdown, and a human-LLM-co-authored schema document that defines conventions.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.65
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 0.95
  inferred: 0.05
  ambiguous: 0.0
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: implements
  - target: "[[entities/obsidian]]"
    type: uses
---

# Three-Layer Architecture

The structural decomposition that the [[concepts/llm-wiki-pattern]] depends on. Three layers, each with a single owner and a single responsibility.

## The three layers

### 1. Raw sources

Curated source documents — articles, papers, images, data files, web clippings.

- **Immutable.** The LLM reads from this layer but never modifies it. ^[extracted]
- **Source of truth.** Every claim in the wiki should trace back here.
- **User-owned.** You decide what gets added.

### 2. The wiki

A directory of LLM-generated markdown files: summaries, entity pages, concept pages, comparisons, an overview, a synthesis.

- **LLM-owned entirely.** The LLM creates, updates, cross-references, and maintains consistency.
- **You read it; the LLM writes it.** ^[extracted]
- Built from category directories (`concepts/`, `entities/`, `skills/`, `references/`, `synthesis/`, `journal/`, `projects/` in this vault).

### 3. The schema

A configuration document — `CLAUDE.md` for Claude Code, `AGENTS.md` for Codex, `SKILL.md` files for skill-based agents — that tells the LLM how the wiki is structured, what conventions apply, and what workflows to follow when ingesting, querying, or maintaining. ^[extracted]

- **Co-evolved by user and LLM.** ^[extracted]
- **What turns the LLM into a disciplined wiki maintainer rather than a generic chatbot.** ^[extracted]
- In this project, the schema lives in `.claude/skills/` (Claude Code skills) and `.hermes/skills/` (Hermes skills), plus this vault's frontmatter conventions.

## Layer flow

```
raw sources    ──read──►   LLM   ──write──►   wiki
                            ▲                    │
                            │                    │
                            └─ guided by ─ schema
```

The schema gates how the LLM moves information from sources into the wiki. Without it, the LLM produces inconsistent output; with it, you get a maintainable knowledge graph.

## Why the separation matters

Conflating any two layers breaks the pattern:

- **Raw + wiki conflated** → no curation; the wiki becomes a dump and synthesis stops compounding ^[inferred]
- **Wiki + schema conflated** → conventions drift across pages; the LLM forgets how to maintain consistency ^[inferred]
- **Raw + schema conflated** → instructions hidden inside source content; opens the door to prompt injection (see the "Content Trust Boundary" section in this vault's `wiki-ingest` skill) ^[inferred]

## See also

- [[concepts/llm-wiki-pattern]] — the parent pattern
- [[concepts/persistent-knowledge-graph]] — what the wiki layer produces over time
- [[skills/index-and-log-files]] — special files that anchor the wiki layer
