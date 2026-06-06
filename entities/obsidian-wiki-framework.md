---
title: obsidian-wiki (framework)
category: entities
tags:
  - tool
  - framework
  - knowledge-management
  - wiki
url: https://github.com/Ar9av/obsidian-wiki
summary: An agent-agnostic framework that ships skill bundles for many AI coding agents, letting them build and maintain a personal knowledge base in Obsidian. The upstream of this vault.
sources:
  - "https://github.com/Ar9av/obsidian-wiki"
created: 2026-06-06T12:44:17Z
updated: 2026-06-06T12:44:17Z
base_confidence: 0.6
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 0.7
  inferred: 0.25
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: implements
  - target: "[[concepts/three-layer-architecture]]"
    type: implements
  - target: "[[entities/obsidian]]"
    type: uses
  - target: "[[references/karpathy-llm-wiki-essay]]"
    type: derived_from
---

# obsidian-wiki (framework)

> A framework enabling AI coding agents to build and maintain a personal knowledge management system using Obsidian as the viewer and LLMs as maintainers.

The upstream project — `Ar9av/obsidian-wiki` — that the **MeteorDreamWiki** vault sits on. Every skill file under `.claude/skills/`, `.hermes/skills/`, and `.agents/skills/` in this repo is sourced from there (pinned by `skills-lock.json`).

**Repository:** <https://github.com/Ar9av/obsidian-wiki>

## What it actually provides

- **Skill bundles** — ~38 skills implementing [[skills/wiki-operations-trinity|Ingest / Query / Lint]] for many agents
- **Vault scaffolding** — `wiki-setup` produces the canonical category structure (`concepts/`, `entities/`, ..., `_raw/`, `_staging/`, `_archives/`) and special files (`index.md`, `log.md`, `hot.md`, `.manifest.json`)
- **Manifest-based delta tracking** — `.manifest.json` records every ingested source by content hash, so subsequent runs only process actual changes
- **Provenance discipline** — `^[extracted]` / `^[inferred]` / `^[ambiguous]` markers and a frontmatter `provenance:` block on every page
- **Optional QMD search backend** — when configured, hybrid BM25+vector retrieval over the wiki itself (see [[entities/qmd]])

## Installation methods

The framework ships three ways to bootstrap a fresh vault:

| Method | Command | When to use |
|---|---|---|
| **pip** | `pip install obsidian-wiki` then `obsidian-wiki setup --vault /path/to/vault` | Recommended for end users |
| **Skills CLI** | `npx skills add Ar9av/obsidian-wiki` | Drop skills into an existing project |
| **git clone + setup.sh** | `git clone …; bash setup.sh` | Track upstream changes directly |

After installation, `/wiki-setup` (in any supported agent) finishes the vault scaffolding.

## Supported agents

Per the upstream README, skill bundles exist for:

- **Claude Code** — discovers skills via `.claude/skills/`
- **Cursor / Windsurf / generic** — discovers via `.agents/skills/`
- **Hermes** — discovers via `.hermes/skills/`
- **Codex / GitHub Copilot CLI / Pi (OpenCode) / OpenClaw** — primarily history mining (read user's `~/.codex`, `~/.copilot`, etc. and ingest)
- **Gemini CLI, Kiro** — also supported per upstream, paths agent-specific ^[ambiguous]

The vault content itself is plain Markdown — switching agents only changes which skill files load, not the data. ^[inferred]

## How ingest is decomposed (four stages)

Per the upstream framework documentation, the [[skills/wiki-operations-trinity|Ingest]] operation is internally a four-stage pipeline:

1. **Ingest** — read source material directly (markdown, PDFs, chat exports, images)
2. **Pull Information** — extract concepts, entities, claims, relationships
3. **Merge** — integrate new knowledge into existing pages, never replace
4. **Schema** — keep frontmatter, tag taxonomy, and relationship types coherent across the graph

This is a refinement of the [[concepts/llm-wiki-pattern|three-operation pattern]] — the "Ingest" primitive in Karpathy's essay is itself a multi-step process when implemented seriously.

## Two portable global skills

Two skills are designed to work **across project boundaries** (not just inside the vault):

- **`/wiki-update`** — *push.* From any project, distill its learnings into the vault.
- **`/wiki-query`** — *pull.* From any project, ask a question against the vault.

Together they make the vault a **long-term memory layer** that's accessible from any project context. ^[inferred]

## Notable features (from the upstream README)

- Project-based organization with cross-references
- Archive and rebuild capabilities (`/wiki-rebuild`, `/wiki-export`, `/wiki-import`)
- Multi-agent history mining (Claude, Codex, Hermes, Pi, OpenClaw, Copilot)
- Automated cross-linking (`/cross-linker`) and tag taxonomy enforcement
- Provenance tracking distinguishing extracted, inferred, and ambiguous claims
- Optional semantic search via [[entities/qmd|QMD]] integration
- Graph visualization and color-coding (`/graph-colorize`)
- Comprehensive linting for broken links and orphaned pages (`/wiki-lint`)

## Open questions / not yet sourced

- The upstream's authorship (one maintainer? a small group?) is not documented in this vault yet ^[ambiguous]
- Licensing of the framework itself — the README mentions it but the exact license terms haven't been pulled into a wiki page ^[ambiguous]
- Roadmap / version cadence ^[ambiguous]

## See also

- [[concepts/llm-wiki-pattern]] — the methodology this framework implements
- [[concepts/three-layer-architecture]] — raw / wiki / schema, mapped onto this framework's folders
- [[skills/wiki-operations-trinity]] — Ingest / Query / Lint, with the four-stage Ingest expanded above
- [[entities/obsidian]] — the viewer half of "Obsidian is the IDE; the LLM is the programmer"
- [[entities/qmd]] — the optional search backend
- [[references/karpathy-llm-wiki-essay]] — the founding pattern
