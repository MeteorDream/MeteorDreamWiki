---
title: Hot Cache
updated: 2026-06-06T12:53:59Z
---

# Hot Cache

*A ~500-word semantic snapshot of recent activity. Updated after every major write operation.*

## Recent Activity

- **[2026-06-06T12:53:59Z] STAGE_COMMIT** — Promoted all 4 staged files. New page `entities/obsidian-wiki-framework.md` is now live (core tier). Three patches applied: [[concepts/llm-wiki-pattern]] gained a "How it's implemented in practice" subsection + relationship to the framework; [[skills/wiki-operations-trinity]] gained the four-stage Ingest decomposition; [[entities/obsidian]] gained a "How agents drive it" subsection. `_staging/` is now clean.
- **[2026-06-06T12:44:17Z] INGEST (staged)** — Distilled the upstream `Ar9av/obsidian-wiki` README. Updated `README.md` and `README-zh.md` with installation methods, the four-stage Ingest pipeline, an agent-compatibility table, and the two portable global skills (`/wiki-update`, `/wiki-query`).
- **[2026-06-06T10:16:21Z] INGEST** — Promoted Karpathy's *LLM Wiki* essay from `_raw/`. Created the founding 12 pages.

## Active Threads

- **Self-referential foundation complete.** The vault now documents both the *pattern* (Karpathy's essay) and the *framework that implements it* (`Ar9av/obsidian-wiki`). The framework page is the new structural hub linking concepts → skills → tools.
- **Staging queue empty.** All 4 staged files have been processed cleanly. Future ingests with `WIKI_STAGED_WRITES=false` (current setting) will write directly.
- **Cross-link audit due.** With a new core-tier hub page (`obsidian-wiki-framework`), running `/cross-linker` would surface any pages that should now reference it but don't.

## Key Takeaways

- **The framework is agent-agnostic.** Skill bundles exist for Claude Code, Cursor, Windsurf, Codex, Gemini CLI, Pi, Kiro, Hermes, OpenClaw, Copilot. Vault content stays plain markdown; only skill loaders change.
- **Ingest is internally a 4-stage pipeline.** Read → Pull → Merge → Schema. The "Schema" stage is what prevents drift over hundreds of pages.
- **Two skills work cross-project.** `/wiki-update` (push) and `/wiki-query` (pull) treat the vault as a long-term memory layer reachable from any project directory.
- **Three install options.** `pip install obsidian-wiki`, `npx skills add Ar9av/obsidian-wiki`, or `git clone + bash setup.sh` — pick by how tightly you want to track upstream.
- **Obsidian fits because it doesn't need an API.** Vault format is plain markdown + `.obsidian/` config; agents write `.md` files directly and Obsidian picks them up live.

## Flagged Contradictions

*None yet.*
