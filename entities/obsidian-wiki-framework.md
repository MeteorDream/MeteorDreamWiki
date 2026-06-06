---
title: obsidian-wiki (framework)
category: entities
tags:
  - tool
  - framework
  - knowledge-management
  - wiki
url: https://github.com/Ar9av/obsidian-wiki
summary: An agent-agnostic framework that ships skill bundles for ~17 AI coding agents, letting them build and maintain a personal knowledge base in Obsidian. The upstream of this vault.
sources:
  - "https://github.com/Ar9av/obsidian-wiki"
  - "agent:wiki-ingest obsidian-wiki/README.md (raw, 32188 bytes)"
created: 2026-06-06T12:44:17Z
updated: 2026-06-06T13:29:22Z
base_confidence: 0.85
lifecycle: stable
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 0.88
  inferred: 0.10
  ambiguous: 0.02
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: implements
  - target: "[[concepts/three-layer-architecture]]"
    type: implements
  - target: "[[entities/obsidian]]"
    type: uses
  - target: "[[references/karpathy-llm-wiki-essay]]"
    type: derived_from
  - target: "[[references/obsidian-wiki-readme]]"
    type: related_to
  - target: "[[skills/agent-bootstrap-discovery]]"
    type: implements
  - target: "[[entities/kepano-obsidian-skills]]"
    type: related_to
  - target: "[[entities/qmd]]"
    type: uses
---

# obsidian-wiki (framework)

> A knowledge mgmt system inspired by the gist published by Andrej Karpathy about maintaining a personal knowledge base with LLMs: the "LLM Wiki" pattern.

The upstream project — `Ar9av/obsidian-wiki` — that the **MeteorDreamWiki** vault sits on. Every skill file under `.claude/skills/`, `.hermes/skills/`, and `.agents/skills/` in this repo originated there (pinned by `skills-lock.json`).

**Repository:** <https://github.com/Ar9av/obsidian-wiki>
**Author / X:** [@_ar9av](https://x.com/_ar9av)
**Founding inspiration:** [[references/karpathy-llm-wiki-essay]]

## Quick install

The framework ships three install paths, all setup-equivalent:

```bash
# A — pip (recommended for end users)
pip install obsidian-wiki
obsidian-wiki setup --vault /path/to/vault

# B — Skills CLI (drop skills into an existing project)
npx skills add Ar9av/obsidian-wiki

# C — git clone (track upstream changes directly)
git clone https://github.com/Ar9av/obsidian-wiki
cd obsidian-wiki && bash setup.sh
```

The pip path is special: `obsidian-wiki setup` writes a config to `~/.obsidian-wiki/config` and installs every wiki skill into all your AI agents (Claude Code, Cursor, Codex, Gemini, Hermes, Pi, etc.). **Skills are symlinked to the installed package by default**, so `pip install -U obsidian-wiki` upgrades them everywhere — re-run `obsidian-wiki setup` to pick up new skills. Use `--copy` instead of symlinks when you want to freeze a particular version.

```bash
obsidian-wiki list              # list bundled skills
obsidian-wiki info              # show install paths, version, config
obsidian-wiki setup --project . # also drop project-local skills + AGENTS.md
obsidian-wiki setup --copy      # copy instead of symlinking
```

`OBSIDIAN_VAULT_PATH` can be a fresh empty directory or an existing Obsidian vault. ^[extracted]

After install, open any project in your agent and say **"set up my wiki"** — that triggers `/wiki-setup`. ^[extracted]

## Agent compatibility (verbatim from the README)

Skills work with **any AI coding agent that can read files.** Each agent has its own conventions for (a) the bootstrap file it reads at startup and (b) where it discovers skills. The framework ships ~17 agent-specific entry points; see [[skills/agent-bootstrap-discovery]] for the full mapping. The most important rows:

| Agent | Bootstrap | Skills directory | Slash commands |
|---|---|---|---|
| Claude Code | `CLAUDE.md` | `.claude/skills/` + `~/.claude/skills/` | ✅ `/wiki-ingest` etc. |
| Cursor | `.cursor/rules/obsidian-wiki.mdc` | `.cursor/skills/` | ✅ |
| Windsurf | `.windsurf/rules/obsidian-wiki.md` | `.windsurf/skills/` | ✅ via Cascade |
| Codex (OpenAI) | `AGENTS.md` | `~/.codex/skills/` | `$wiki-ingest` (Codex uses `$`) ^[extracted] |
| Gemini CLI | `GEMINI.md` | `~/.gemini/skills/` | ✅ |
| Google Antigravity | `.agent/rules/` + `.agent/workflows/` | `.agents/skills/` | ✅ |
| Kiro IDE/CLI | `.kiro/steering/obsidian-wiki.md` | `.kiro/skills/` + `~/.kiro/skills/` | ✅ |
| Hermes | `.hermes.md` | `~/.hermes/skills/` | ✅ |
| OpenClaw | `AGENTS.md` | `~/.openclaw/skills/` + `~/.agents/skills/` | ✅ |
| OpenCode / Aider / Factory Droid | `AGENTS.md` | `~/.agents/skills/` | varies |
| Trae / Trae CN | `AGENTS.md` | `~/.trae/skills/` / `~/.trae-cn/skills/` | ✅ via Agent tool |
| GitHub Copilot (VS Code) | `.github/copilot-instructions.md` | — | describe in chat |
| GitHub Copilot (CLI) | — | `~/.copilot/skills/` | ✅ |
| Kilocode | `AGENTS.md` / `CLAUDE.md` | `.agents/skills/` + `.claude/skills/` | ✅ |
| Pi | `AGENTS.md` | `.pi/skills/` + `~/.pi/agent/skills/` | ✅ |

> Each agent has its own convention for discovering skills. `setup.sh` symlinks the canonical `.skills/` directory into each agent's expected location. **You write skills once, every agent can use them.** ^[extracted]

In **MeteorDreamWiki** specifically, three of these are committed: `.claude/`, `.hermes/`, `.agents/`. The others are dormant slots — drop a skill bundle in and the corresponding agent picks them up.

## How ingest works (four stages — verbatim)

The README documents Ingest as a four-stage pipeline. ^[extracted]

1. **Ingest** — *"The agent reads your source material directly. It handles whatever you throw at it: markdown files, PDFs (with page ranges), JSONL conversation exports, plain text logs, chat exports, meeting transcripts, and images (screenshots, whiteboard photos, diagrams — vision-capable model required). No preprocessing step, no pipeline to run. The agent reads the file the same way it reads code."*
2. **Pull Information** — *"From the raw source, the agent pulls out concepts, entities, claims, relationships, and open questions. A conversation about debugging a React hook yields a 'stale closure' pattern. A research paper yields the key idea and its caveats. A work log yields decisions and their rationale. Noise gets dropped, signal gets kept. Each page also gets a 1–2 sentence `summary:` in its frontmatter at write time — later queries use this to preview pages without opening them."*
3. **Merge** — *"New knowledge gets merged against what's already in the wiki. If a concept page exists, the agent updates it — merging new information, noting contradictions, strengthening cross-references. If it's genuinely new, a page gets created. Nothing is duplicated. Sources are tracked in frontmatter so every claim stays attributable."*
4. **Schema** — *"The wiki schema isn't fixed upfront. It emerges from your sources and evolves as you add more. The agent maintains coherence: categories stay consistent, wikilinks point to real pages, the index reflects what's actually there. When you add a new domain (a new project, a new field of study), the schema expands to accommodate it without breaking what exists."*

The first stage is data IO. The middle two are the cognitive work. The last one is what prevents the graph from drifting as it grows. ^[inferred]

This is a refinement of the three-operation pattern in [[skills/wiki-operations-trinity]] — the "Ingest" primitive in Karpathy's essay is itself a multi-step process when implemented seriously. ^[inferred]

## What the framework adds on top of Karpathy's pattern

Per the README's own "What we added on top of Karpathy's pattern" section:

- **Delta tracking** — `.manifest.json` ledger; subsequent runs process only what's new or changed
- **Project-based organization** — knowledge gets filed under projects when project-specific, globally when not
- **Archive and rebuild** — timestamped snapshots, full restore from any prior archive (`/wiki-rebuild`)
- **Multi-agent ingest** — dedicated history miners for Claude, Codex, Hermes, OpenClaw, Pi; plus Windsurf data, ChatGPT exports, Slack logs, meeting transcripts; catch-all `/data-ingest` for arbitrary text
- **Cross-agent targeted search** — `/wiki-claude`, `/wiki-codex`, `/wiki-hermes`, `/wiki-openclaw`, `/wiki-copilot`, `/wiki-pi` find topic-specific sessions in *one specific agent's* history and ingest just those
- **Audit and lint** — orphans, broken links, stale content, contradictions, missing frontmatter
- **Automated cross-linking** — `/cross-linker` weaves new pages into the existing graph
- **Tag taxonomy** — controlled vocabulary in `_meta/taxonomy.md`
- **Provenance tracking** — `^[extracted]` / `^[inferred]` / `^[ambiguous]` markers + per-page `provenance:` frontmatter block
- **Multimodal sources** — images, screenshots, slide captures (vision model required)
- **Wiki insights** — top hubs, bridge pages, tag cluster cohesion, surprising connections, graph delta since last run, written to `_insights.md`
- **Graph export** — `/wiki-export` produces graph.json (queryable), graph.graphml (Gephi/yEd), cypher.txt (Neo4j), and a self-contained interactive `graph.html`
- **Tiered retrieval** — `wiki-query` reads titles + summaries first, opens page bodies only when the cheap pass can't answer; "quick answer" forces index-only
- **QMD semantic search (optional)** — see [[entities/qmd]]
- **`_raw/` staging directory** — drop rough notes, the next ingest promotes them; in this vault, **this rule was extended to archive promoted files to `source/`** (the upstream's default is to remove them)

## Two portable "global" skills

`/wiki-update` and `/wiki-query` are explicitly designed to work **from any project, not just inside the vault**. The README walks through this verbatim:

> You're in some project, say `~/projects/my-cool-app`, working with Claude or Pi. Two commands:
> ```
> /wiki-update         # write to the wiki: distill what you've learned
> /wiki-query <topic>  # read from the wiki: pull context about anything you've captured before
> ```

`/wiki-update` reads your project, figures out what's worth keeping, and distills it into the vault — *"architecture decisions, patterns you discovered, key concepts, trade-offs you evaluated. It doesn't copy code or dump file listings. It distills the stuff you'd forget in 3 months."* On reruns it diffs against `git log` and only processes the delta. ^[extracted]

`/wiki-query` *"goes the other direction. You're working on something and you want to know what your wiki says about a topic. Maybe you solved a similar problem 2 months ago in a different project and the answer is already in your vault."* ^[extracted]

## Project structure (canonical layout — upstream)

The upstream repo is laid out as a **canonical-source-symlinked-everywhere** pattern:

```
.skills/                  ← canonical SKILL.md definitions (source of truth)
├── wiki-setup/SKILL.md
├── wiki-ingest/SKILL.md
├── … (~30 more)

# Per-agent symlinks created by setup.sh:
.claude/skills/   →  symlinks into .skills/*
.cursor/skills/   →  symlinks into .skills/*
.windsurf/skills/ →  symlinks into .skills/*
.agents/skills/   →  symlinks into .skills/*
.pi/skills/       →  symlinks into .skills/*
.kiro/skills/     →  symlinks into .skills/*

# Global symlinks (~/) for cross-project use:
~/.claude/skills/, ~/.gemini/skills/, ~/.codex/skills/,
~/.hermes/skills/, ~/.openclaw/skills/, ~/.copilot/skills/,
~/.trae/skills/, ~/.trae-cn/skills/, ~/.kiro/skills/,
~/.pi/agent/skills/, ~/.agents/skills/

# Bootstrap files (per-agent project context):
CLAUDE.md, GEMINI.md, AGENTS.md, .hermes.md,
.cursor/rules/obsidian-wiki.mdc, .windsurf/rules/obsidian-wiki.md,
.kiro/steering/obsidian-wiki.md, .agent/rules/obsidian-wiki.md,
.agent/workflows/obsidian-wiki.md, .github/copilot-instructions.md
```

> **MeteorDreamWiki diverges here.** Instead of one canonical `.skills/` dir with symlinks, this vault commits the actual files in `.claude/`, `.hermes/`, `.agents/` (verified byte-identical via SHA-256). That makes local edits possible (which we have already done — see `log.md` `CONVENTION_CHANGE` entry from 2026-06-06T13:09Z) at the cost of having to manually sync the three trees. ^[inferred]

## Companion skills (kepano)

The README explicitly recommends installing [`kepano/obsidian-skills`](https://github.com/kepano/obsidian-skills) alongside this framework. They use the same Agent Skills spec so they coexist with no conflicts. See [[entities/kepano-obsidian-skills]].

## Visualization

`graph-colorize` rewrites `<vault>/.obsidian/graph.json` so Obsidian tints nodes by tag, folder, or visibility. It backs up the existing graph.json first and only touches the `colorGroups` field — zoom, physics, and filter preferences stay intact. Modes: `by-tag` (default — top 10 tags), `by-category` (the seven vault folders), `by-visibility` (`visibility/pii` and `visibility/internal`), `combined`, or `custom`. ^[extracted]

## Quoted lines worth preserving

> Each agent has its own convention for discovering skills. `setup.sh` symlinks the canonical `.skills/` directory into each agent's expected location. You write skills once, every agent can use them. ^[extracted]

> The wiki schema isn't fixed upfront. It emerges from your sources and evolves as you add more. ^[extracted]

> If a concept page already exists in the vault, [`/wiki-update` and `/wiki-query`] merges into it. Everything gets cross-linked with `[[wikilinks]]`, tracked in `.manifest.json`, and logged. ^[extracted]

## Open questions / not yet sourced

- Authorship (one maintainer @_ar9av visible; team size unknown) ^[ambiguous]
- Licensing of the framework itself — README doesn't state a license verbatim ^[ambiguous]
- Roadmap / version cadence ^[ambiguous]

## See also

- [[concepts/llm-wiki-pattern]] — the methodology this framework implements
- [[concepts/three-layer-architecture]] — raw / wiki / schema, mapped onto this framework's folders
- [[skills/wiki-operations-trinity]] — Ingest / Query / Lint, with the four-stage Ingest expanded above
- [[skills/agent-bootstrap-discovery]] — the multi-file bootstrap pattern this framework implements
- [[entities/obsidian]] — the viewer half of "Obsidian is the IDE; the LLM is the programmer"
- [[entities/qmd]] — the optional search backend
- [[entities/kepano-obsidian-skills]] — recommended companion skills
- [[references/obsidian-wiki-readme]] — the README this page was built from
- [[references/karpathy-llm-wiki-essay]] — the founding pattern
