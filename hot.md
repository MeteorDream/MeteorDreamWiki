---
title: Hot Cache
updated: 2026-06-06T10:16:21Z
---

# Hot Cache

*A ~500-word semantic snapshot of recent activity. Updated after every major write operation.*

## Recent Activity

- **[2026-06-06T10:16:21Z] INGEST** — Promoted Karpathy's *LLM Wiki* essay from `_raw/`. Created 12 pages: 4 concepts ([[concepts/llm-wiki-pattern|llm-wiki-pattern]], [[concepts/three-layer-architecture|three-layer-architecture]], [[concepts/rag-vs-llm-wiki|rag-vs-llm-wiki]], [[concepts/persistent-knowledge-graph|persistent-knowledge-graph]]), 4 entities ([[entities/andrej-karpathy|karpathy]], [[entities/vannevar-bush|bush]], [[entities/obsidian|obsidian]], [[entities/qmd|qmd]]), 2 skills ([[skills/wiki-operations-trinity|operations-trinity]], [[skills/index-and-log-files|index-and-log-files]]), 2 references ([[references/karpathy-llm-wiki-essay|karpathy-essay]], [[references/memex|memex]]). Updated `index.md` and `.manifest.json`. The vault now contains its own founding theory.
- **[2026-06-06T09:47:38Z] INIT** — Vault created at project root (.) by `/wiki-setup`. 7 category dirs, 3 special files, `.env`, `.gitignore`, `README.md`.

## Active Threads

- **Founding self-reference.** The first ingest is the essay that defines this vault's own pattern — the [[concepts/llm-wiki-pattern]] now has a wiki page *about* the pattern that produced it. This is the seed; all future ingests inherit a graph rooted here.
- **Source pipeline configuration pending.** `OBSIDIAN_SOURCES_DIR=.` is currently the repo root, which is mostly skill definitions. The user should either point this at real notes/papers, or rely on `/claude-history-ingest`, `/hermes-history-ingest`, `/data-ingest`, and `/ingest-url` for future sources.

## Key Takeaways

- **The wiki is a persistent, compounding artifact.** Cross-references already exist; contradictions have already been flagged; synthesis already reflects everything ingested. ([[references/karpathy-llm-wiki-essay]])
- **Three layers, three owners.** Raw sources are user-curated and immutable; the wiki is LLM-owned; the schema is co-evolved. ([[concepts/three-layer-architecture]])
- **Bookkeeping is the bottleneck humans abandon.** LLMs don't get bored. The cost of maintenance approaches zero, which is why the pattern works at all. ([[concepts/llm-wiki-pattern]])
- **"Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."** ([[entities/andrej-karpathy]])
- **Memex was the 1945 vision; the LLM solves the missing piece — maintenance.** ([[references/memex]])

## Flagged Contradictions

*None yet — the vault has only one source so far. Contradictions surface when sources disagree.*
