---
title: Agent Bootstrap & Skill Discovery
category: skills
tags:
  - workflow
  - agent
  - skill
  - integration
summary: The multi-file convention by which different AI coding agents discover their bootstrap context (CLAUDE.md, AGENTS.md, GEMINI.md, etc.) and their skill bundles (.claude/skills/, .agents/skills/, ~/.codex/skills/, etc.). Mapping this is what lets one canonical skill set work across ~17 agents.
sources:
  - "https://github.com/Ar9av/obsidian-wiki/blob/main/README.md"
created: 2026-06-06T13:29:22Z
updated: 2026-06-06T13:29:22Z
base_confidence: 0.7
lifecycle: stable
lifecycle_changed: 2026-06-06
tier: core
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[entities/obsidian-wiki-framework]]"
    type: implements
  - target: "[[concepts/three-layer-architecture]]"
    type: implements
---

# Agent Bootstrap & Skill Discovery

Every AI coding agent has two pickup points it scans on startup:

1. **A bootstrap file** — markdown that defines project-level context. Read once per session.
2. **A skills directory** — markdown SKILL.md files that load on demand when the user asks for them.

Different agents pick different filenames and paths for both. The [[entities/obsidian-wiki-framework]] framework's `setup.sh` handles all of them by symlinking a single canonical `.skills/` directory into each agent's expected location. **One source of truth, every agent supported.**

## The full mapping (per the framework's README)

| Agent | Bootstrap file | Skill discovery path | Slash-command convention |
|---|---|---|---|
| Claude Code | `CLAUDE.md` | `.claude/skills/` + `~/.claude/skills/` | `/skill-name` |
| Cursor | `.cursor/rules/<name>.mdc` (with `alwaysApply: true`) | `.cursor/skills/` | `/skill-name` |
| Windsurf | `.windsurf/rules/<name>.md` | `.windsurf/skills/` | via Cascade chat |
| Codex (OpenAI) | `AGENTS.md` | `~/.codex/skills/` | **`$skill-name`** ^[extracted] |
| Gemini CLI | `GEMINI.md` | `~/.gemini/skills/` | `/skill-name` |
| Google Antigravity | `.agent/rules/` + `.agent/workflows/` | `.agents/skills/` | via workflows registry |
| Kiro IDE/CLI | `.kiro/steering/<name>.md` (with `inclusion: always`) | `.kiro/skills/` + `~/.kiro/skills/` | `/skill-name` |
| Hermes | `.hermes.md` (then falls back to `AGENTS.md`) | `~/.hermes/skills/` | `/skill-name` |
| OpenClaw | `AGENTS.md` (priority 10) | `~/.openclaw/skills/` + `~/.agents/skills/` | `/skill-name` |
| OpenCode / Aider / Factory Droid / Trae | `AGENTS.md` | `~/.agents/skills/` (Trae also `~/.trae/skills/`) | varies |
| GitHub Copilot (VS Code) | `.github/copilot-instructions.md` | — | describe in chat |
| GitHub Copilot (CLI) | — | `~/.copilot/skills/` | `/skill-name` |
| Kilocode | `AGENTS.md` / `CLAUDE.md` | `.agents/skills/` + `.claude/skills/` | `/skill-name` |
| Pi (OpenCode) | `AGENTS.md` (walks up from cwd) | `.pi/skills/`, `.agents/skills/`, `~/.pi/agent/skills/` | `/skill-name` |

## Patterns visible in this table

Reading the table, several conventions emerge:

- **`AGENTS.md` is the de facto cross-agent standard.** Codex, OpenCode, Aider, Factory Droid, Trae, Hermes (fallback), OpenClaw, Kilocode, and Pi all read it. Skills aimed at "any compliant agent" should write `AGENTS.md` rather than agent-specific files. ^[inferred]
- **`.agents/skills/` is the de facto cross-agent skills directory** for the same reason. ^[inferred]
- **Some agents prefer global skills** (`~/.codex/skills/`, `~/.hermes/skills/`, etc.) over project-local. The framework symlinks both wherever possible so skills are reachable from any project. ^[inferred]
- **Per-agent quirks:**
  - Codex uses `$` instead of `/` for slash commands ^[extracted]
  - Cursor's bootstrap file uses `.mdc` (with `alwaysApply: true` frontmatter) ^[extracted]
  - Kiro uses `inclusion: always` in `.kiro/steering/` ^[extracted]
  - Antigravity splits "rules" (always-on) from "workflows" (slash-command registry) into two dirs ^[extracted]

## How MeteorDreamWiki implements this

This vault commits skill files in three of the agent paths directly:

```
.claude/skills/   ← Claude Code (committed, byte-identical to the others)
.hermes/skills/   ← Hermes (committed, byte-identical)
.agents/skills/   ← OpenCode / Aider / Trae / Pi / OpenClaw / Cursor (committed, byte-identical)
```

The other paths are dormant. To add Cursor support, for example: drop `.cursor/skills/` (or symlink `.skills/*` into it), and write `.cursor/rules/obsidian-wiki.mdc`.

The three trees are kept in lockstep manually — when a skill is edited in `.claude/`, it's `cp`-ed to `.hermes/` and `.agents/`. SHA-256 hash equality is the verification (see `log.md` for the most recent CONVENTION_CHANGE entry; both `wiki-ingest` and `wiki-setup` were synced this way on 2026-06-06).

> **Why not symlinks?** The upstream uses symlinks from one canonical `.skills/` dir. This vault committed actual files instead because we make local edits (e.g., the archive-don't-delete rule introduced 2026-06-06). Symlinks would point back to upstream and lose our edits on the next `pip install -U`. The tradeoff: manual three-way sync. ^[inferred]

## Two portable "global" skills

Per the framework's design, two skills live in `~/<agent>/skills/` (the global, cross-project location) so they work from any project, not just inside the vault:

- `/wiki-update` — push from current project into the vault
- `/wiki-query` — pull from the vault into current project

This pattern is the bridge between the project layer and the [[concepts/three-layer-architecture|wiki layer]] — the only two skills that explicitly cross that boundary. ^[inferred]

## See also

- [[entities/obsidian-wiki-framework]] — the framework that implements this
- [[concepts/three-layer-architecture]] — schema layer concretized as bootstrap files + skill paths
- [[skills/wiki-operations-trinity]] — what those skill files actually do
- [[references/obsidian-wiki-readme]] — the source of this mapping
