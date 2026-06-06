---
title: "obsidian-wiki — README"
category: references
tags:
  - reference
  - source
  - knowledge-management
  - framework
url: https://github.com/Ar9av/obsidian-wiki/blob/main/README.md
summary: The README of the obsidian-wiki framework — the canonical citation source for everything this vault knows about how the framework works, what agents it supports, and how it extends Karpathy's LLM Wiki pattern.
sources:
  - "https://github.com/Ar9av/obsidian-wiki/blob/main/README.md"
created: 2026-06-06T13:29:22Z
updated: 2026-06-06T13:29:22Z
base_confidence: 0.85
lifecycle: stable
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 1.0
  inferred: 0.0
  ambiguous: 0.0
relationships:
  - target: "[[entities/obsidian-wiki-framework]]"
    type: related_to
  - target: "[[references/karpathy-llm-wiki-essay]]"
    type: related_to
---

# obsidian-wiki — README

The README of the [[entities/obsidian-wiki-framework|`Ar9av/obsidian-wiki`]] framework. This page is the **provenance anchor** — anything in this vault tagged "from the framework README" traces back here.

## Type
GitHub README markdown, 32 KB, ~477 lines as of 2026-06-06.
Source URL: <https://github.com/Ar9av/obsidian-wiki/blob/main/README.md>

## What it covers

| Section | What it establishes |
|---|---|
| Intro | Self-positioning relative to Karpathy's [[references/karpathy-llm-wiki-essay\|LLM Wiki essay]] |
| Quick Start | Three install paths: pip, Skills CLI (`npx skills add`), git clone + `setup.sh` |
| Agent Compatibility | Comprehensive table of ~17 supported agents with bootstrap files + skill discovery dirs |
| How it works | Four-stage Ingest pipeline: Ingest → Pull Information → Merge → Schema |
| What we added on top | Delta tracking, project orgs, archives, multi-agent ingest, cross-agent search, lint, cross-linking, tag taxonomy, provenance, multimodal, insights, graph export, tiered retrieval, optional QMD |
| Visualization | Obsidian Graph View + `graph-colorize` skill |
| Optional QMD | Setup steps and `QMD_*` env vars |
| `_raw/` | Staging directory convention (upstream's default removes promoted files; **this vault diverges** — see `CONVENTION_CHANGE` in `log.md` 2026-06-06T13:09Z) |
| Skills | Full skill catalog (~30 entries) with slash commands |
| Recommended companion | `kepano/obsidian-skills` for Obsidian-format mastery (see [[entities/kepano-obsidian-skills]]) |
| Project Structure | Canonical `.skills/` source-of-truth + per-agent symlinks layout |
| Using from other projects | The `wiki-update` push / `wiki-query` pull pattern |
| Contributing | Skill creation workflow |

## Direct claims worth preserving

- *"You point it at your Obsidian vault and tell it what to do."* — the framework's positioning ^[extracted]
- *"You write skills once, every agent can use them."* — about the canonical-source-of-truth + symlinks design ^[extracted]
- *"The wiki schema isn't fixed upfront. It emerges from your sources and evolves as you add more."* ^[extracted]
- *"`obsidian-wiki setup` writes the config to `~/.obsidian-wiki/config` and installs every wiki skill into all your AI agents."* — defines the per-user config location ^[extracted]
- *"Skills are symlinked to the installed package, so `pip install -U obsidian-wiki` upgrades them everywhere."* — the upgrade story ^[extracted]
- *"Codex uses `$`"* (Codex's slash-command convention is `$` instead of `/`) ^[extracted]

## Local provenance

Originally landed at `_raw/obsidian-wiki.md` on 2026-06-06T20:38 local time (32188 bytes, sha256 `10459667…b3f`). Promoted into this wiki on 2026-06-06T13:29:22Z by `/wiki-ingest`. Per the **archive-don't-delete** rule, the original was archived to `source/llm/frameworks/obsidian-wiki-readme.md` (with archive frontmatter) instead of being deleted. Open that file to see the README verbatim plus its provenance metadata.

This is the second source ever ingested into MeteorDreamWiki. The first was [[references/karpathy-llm-wiki-essay]] — it's worth reading these two together: the essay is the *theory*, this README is the *concrete realization*.

## Wiki pages this source produced or materially updated

- New pages:
  - [[skills/agent-bootstrap-discovery]] — the multi-file bootstrap pattern
  - [[entities/kepano-obsidian-skills]] — the recommended companion skill set
  - this page
- Updated pages:
  - [[entities/obsidian-wiki-framework]] — major rewrite (now backed by direct README content rather than a fetch summary)
  - [[skills/wiki-operations-trinity]] — clarified four-stage Ingest, added portable-skills note
  - [[concepts/persistent-knowledge-graph]] — added `graph-colorize`, `wiki-export` mentions
  - [[entities/qmd]] — added setup commands

## See also

- [[entities/obsidian-wiki-framework]]
- [[references/karpathy-llm-wiki-essay]]
- [[skills/agent-bootstrap-discovery]]
- [[entities/kepano-obsidian-skills]]
