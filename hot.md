---
title: Hot Cache
updated: 2026-06-06T13:29:22Z
---

# Hot Cache

*A ~500-word semantic snapshot of recent activity. Updated after every major write operation.*

## Recent Activity

- **[2026-06-06T13:29:22Z] INGEST** — Distilled the 32 KB upstream `obsidian-wiki` README from `_raw/`. Created 3 new pages ([[references/obsidian-wiki-readme]], [[entities/kepano-obsidian-skills]], [[skills/agent-bootstrap-discovery]]) and substantially rewrote 4 existing ones ([[entities/obsidian-wiki-framework]] now backed by direct README content rather than a fetch summary; [[skills/wiki-operations-trinity]] gained tiered-retrieval + portable-skills sections; [[concepts/persistent-knowledge-graph]] gained graph-colorize/wiki-export coverage; [[entities/qmd]] gained concrete setup commands). First exercise of the archive-don't-delete rule on a real `_raw/` file: archived to `source/llm/frameworks/obsidian-wiki-readme.md`, deleted from `_raw/`, manifest updated.
- **[2026-06-06T13:09:10Z] CONVENTION_CHANGE** — Introduced the **archive-don't-delete** rule.
- **[2026-06-06T12:53:59Z] STAGE_COMMIT** — Promoted all 4 staged files from the prior staged ingest.

## Active Threads

- **The framework page is now authoritative.** [[entities/obsidian-wiki-framework]] no longer relies on the GitHub fetch summary — it's backed by the full README content, with a 17-agent compatibility table and verbatim 4-stage Ingest descriptions. New `^[ambiguous]` markers were retired where direct extraction now applies.
- **`skills/agent-bootstrap-discovery` is a new core hub.** It maps every supported agent's bootstrap file + skill discovery path. Useful when adding a new agent slot to MeteorDreamWiki (currently we have `.claude/`, `.hermes/`, `.agents/` committed — the page documents the full landscape).
- **Two sources, two archive examples.** `source/llm/knowledge/karpathy_llm_wiki.md` (the essay, retroactive) and `source/llm/frameworks/obsidian-wiki-readme.md` (the README, prospective). Both have the now-expected `archived_content_hash` annotation explaining trailing-newline divergence — the convention is working as designed.

## Key Takeaways

- **The framework is ~17 agents, not 4.** Per the README's compatibility table: Claude Code, Cursor, Windsurf, Codex (uses `$` not `/`), Gemini CLI, Antigravity, Kiro, Hermes, OpenClaw, OpenCode, Aider, Factory Droid, Trae/Trae CN, Copilot (VS Code + CLI), Kilocode, Pi.
- **The canonical install pattern is "symlinks from one `.skills/` source-of-truth"**, but MeteorDreamWiki diverges by committing actual files in three trees — the tradeoff is local edits vs. manual sync. The CONVENTION_CHANGE on 2026-06-06T13:09 is the working example of why we made this tradeoff.
- **Two skills are explicitly portable: `/wiki-update` (push) and `/wiki-query` (pull).** They're the only skills that explicitly cross the project↔vault boundary.
- **`AGENTS.md` is the de facto cross-agent standard.** Codex, OpenCode, Aider, Factory Droid, Trae, Hermes (fallback), OpenClaw, Kilocode, Pi all read it. New skills should write `AGENTS.md` not agent-specific files.
- **`defuddle` is an LLM-economy optimization.** kepano's companion skill set strips boilerplate from web pages before the LLM has to read it.

## Flagged Contradictions

*None yet — but to watch:* the wiki's [[concepts/llm-wiki-pattern]] page describes the ingest primitive at one level of abstraction, while the framework documents 4 internal stages. Both reconciled in [[entities/obsidian-wiki-framework]] (the framework page treats the 4 stages as a refinement, not a replacement). Worth re-reading the two pages together if you ever feel they drift.
