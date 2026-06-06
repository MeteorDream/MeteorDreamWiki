---
title: Andrej Karpathy
category: entities
tags:
  - person
  - ai-researcher
  - llm
summary: AI researcher who articulated the LLM Wiki pattern — using LLM agents to incrementally build and maintain personal knowledge bases as interlinked markdown wikis.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.5
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: supporting
provenance:
  extracted: 0.4
  inferred: 0.55
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: related_to
  - target: "[[references/karpathy-llm-wiki-essay]]"
    type: related_to
---

# Andrej Karpathy

AI researcher and educator who authored the essay that defines the [[concepts/llm-wiki-pattern]] this project is built on.

## Why he appears in this wiki

The pattern itself — LLM as a wiki maintainer working alongside the user, with [[entities/obsidian|Obsidian]] as the IDE — is articulated in the essay [[references/karpathy-llm-wiki-essay]]. The architecture, the [[concepts/three-layer-architecture|three layers]], and the [[skills/wiki-operations-trinity|Ingest/Query/Lint trinity]] all come from his framing.

## Working setup (as described in the essay)

> I have the LLM agent open on one side and Obsidian open on the other. The LLM makes edits based on our conversation, and I browse the results in real time — following links, checking the graph view, reading the updated pages. ^[extracted]

This is the canonical configuration: **a side-by-side agent + viewer.** It implies the human stays in the loop on most ingests. The wiki-ingest skill in this vault preserves the option (via `WIKI_STAGED_WRITES=true`) but defaults to direct writes. ^[inferred]

## Notable framing

- **"Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."** ^[extracted] — the analogy that organizes the rest of the essay
- **Bookkeeping is the bottleneck.** Humans abandon wikis because cross-reference maintenance grows faster than value; LLMs don't get bored. ^[extracted]
- **The pattern is intentionally abstract.** Karpathy describes the idea, not an implementation — leaving directory structure, schema conventions, and tooling to each user. ^[extracted]

## Beyond this wiki

Karpathy is also known for foundational work in deep learning, computer vision, and AI education. Those facts are not yet sourced from this vault's ingests — pages should be expanded only when actual sources arrive. ^[ambiguous]

## See also

- [[concepts/llm-wiki-pattern]]
- [[references/karpathy-llm-wiki-essay]]
- [[references/memex]] — the historical antecedent he credits
