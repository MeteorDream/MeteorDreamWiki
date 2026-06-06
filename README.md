# MeteorDreamWiki

*English · [中文](./README-zh.md)*

An [Obsidian](https://obsidian.md) vault wired into the [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki) skill collection — a personal knowledge base where Claude Code (and other AI agents) ingest your notes, conversations, and source documents and **distill them into an interconnected wiki**, instead of re-discovering them on every query.

The repository root *is* the vault. Open this folder in Obsidian and you're done.

This project is an instantiation of [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). The pattern's founding essay has been ingested into this vault — see [[concepts/llm-wiki-pattern]] and [[references/karpathy-llm-wiki-essay]].

> **About the upstream framework.** The skills under `.claude/skills/`, `.hermes/skills/`, and `.agents/skills/` come from [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki) — *"a framework enabling AI coding agents to build and maintain a personal knowledge management system using Obsidian as the viewer and LLMs as maintainers."* The framework is agent-agnostic: it ships skills for Claude Code, Cursor, Windsurf, Codex, Gemini CLI, Pi, Kiro, Hermes, OpenClaw, GitHub Copilot, and others. **MeteorDreamWiki** is one concrete vault built on top of it.

---

## The LLM Wiki methodology

> Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase. — Andrej Karpathy

Most LLM + document workflows look like RAG: upload files, retrieve chunks at query time, generate an answer. This works, but **the LLM rediscovers knowledge from scratch on every question.** Nothing accumulates. Cross-document insights and contradictions have to be re-derived every time.

The LLM Wiki pattern moves the synthesis *upstream* of the query. The LLM **incrementally builds and maintains a persistent wiki** — a structured, interlinked collection of markdown files between you and your raw sources. When a new source arrives, the LLM doesn't just index it for later; it reads it, extracts what's useful, and integrates it into the existing wiki — updating entity pages, revising summaries, flagging contradictions, strengthening synthesis. Knowledge is **compiled once and then kept current**, not re-derived per query.

### Three layers, three owners

```
                                      schema
                                  (skills + .env)
                                         │
                                  guides how the
                                   LLM behaves
                                         │
                                         ▼
   raw sources       ──read──►          LLM         ──write──►       wiki
  (your inputs)                (skills/agents)             (markdown pages)

   user-curated                 stays in the loop          LLM-owned, you read
   immutable                    ingest / query / lint      links, graph, frontmatter
```

| Layer | Owner | What it is | In this vault |
|---|---|---|---|
| **Raw sources** | You | Articles, papers, agent transcripts, web clippings — immutable, the source of truth | `source/` (archived inputs), `_raw/` (transient inbox), `~/.claude`, `~/.codex`, `~/.hermes`, plus anything you add to `OBSIDIAN_SOURCES_DIR` |
| **The wiki** | The LLM | Distilled, interlinked markdown pages with frontmatter, wikilinks, and a maintained graph | `concepts/`, `entities/`, `skills/`, `references/`, `synthesis/`, `journal/`, `projects/` |
| **The schema** | Co-evolved by you and the LLM | The conventions that turn the LLM into a disciplined wiki maintainer rather than a generic chatbot | `.claude/skills/`, `.hermes/skills/`, frontmatter rules, tag taxonomy, `.env` |

> **Why `source/` and `_raw/` are split.** `_raw/` is the *inbox* where new files arrive; `source/` is the *archive* where they live forever after ingestion. The split lets `wiki-ingest` keep one rule simple ("everything in `_raw/` is unprocessed") while preserving every byte of provenance in `source/<topic>/`. You can grep, re-read, or re-ingest archived sources at any time — the vault never silently throws data away.

### The three operations

These three primitives are all you really need. Every skill in this vault is a refinement of one of them.

1. **Ingest** — Drop a source into `_raw/` (or point `OBSIDIAN_SOURCES_DIR` at it). The LLM reads it, writes a summary, updates the index, refreshes 10–15 related pages, and appends a log entry. *A single source touches many pages — that's the whole point.*
2. **Query** — Ask a question. The LLM reads the index, drills into relevant pages, and answers with citations. **Good answers can be filed back into the wiki as new pages**, so exploration also compounds — not just ingestion.
3. **Lint** — Periodically health-check the wiki: contradictions between pages, stale claims that newer sources superseded, orphan pages with no inbound links, important concepts mentioned but lacking their own page, missing cross-references. The LLM is good at suggesting *new* questions and sources to fill gaps.

#### How ingest actually works (four stages, per the upstream framework)

The `wiki-ingest` skill decomposes Ingest into four explicit stages — this is the model the framework documents:

1. **Ingest** — read source material directly. Markdown, PDFs, chat exports, images (vision-capable models only).
2. **Pull Information** — extract concepts, entities, claims, and relationships. Tag each claim as *extracted* / *inferred* / *ambiguous* as it's pulled.
3. **Merge** — integrate the new knowledge into existing pages, never replace. Update summaries, add wikilinks, record the source. A single ingest touches 10–15 pages.
4. **Schema** — keep frontmatter, tag taxonomy, and relationship types coherent as the graph evolves. This is what prevents drift over hundreds of pages.

A `.manifest.json` ledger tracks every source ever ingested (path, content hash, mtime, pages it produced) so subsequent runs only process what's actually new — that's **delta tracking**, and it's what makes append-mode ingest fast on a vault with thousands of sources.

### Two special files keep it navigable

- **`index.md`** — *content-oriented*. A catalog of every page with one-line summaries, organized by category. The LLM reads this first when answering a query, then drills into specific pages. Avoids the need for embedding-based RAG at the scale of a few hundred pages.
- **`log.md`** — *chronological*. Append-only record of ingests, queries, and lint passes. Each entry has a parseable prefix so simple grep gives you a timeline.

This vault adds two more: `hot.md` (a ~500-word semantic snapshot of recent activity, key takeaways, active threads) and `.manifest.json` (machine-readable bookkeeping that lets append-mode ingest skip already-processed sources).

### Why it actually works

The tedious part of maintaining a knowledge base isn't the reading or thinking — **it's the bookkeeping**. Cross-reference updates, summary refreshes, contradiction tracking, consistency across dozens of pages. Humans abandon wikis because the maintenance cost grows faster than the value.

LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass. **The wiki stays maintained because the cost of maintenance is near zero.**

You curate sources, ask good questions, and think about what it all means. The LLM does everything else.

The pattern echoes Vannevar Bush's 1945 [Memex](https://en.wikipedia.org/wiki/Memex) vision — a personal, curated knowledge store with associative trails between documents. Bush identified the right *form*; the LLM finally solves the *labor* problem.

---

## What lives here

| Path | Purpose |
|---|---|
| `concepts/` | Distilled ideas, theories, mental models |
| `entities/` | People, products, libraries, organizations |
| `skills/` | How-to knowledge and procedures |
| `references/` | External sources, URLs, citations |
| `synthesis/` | Cross-cutting pages that connect multiple concepts |
| `journal/` | Time-anchored notes |
| `projects/` | Per-project knowledge (populated during ingest) |
| `source/` | **Permanent archive of every raw input ever ingested**, organized by topic (e.g. `source/llm/knowledge/`, `source/web-clippings/<host>/`, `source/transcripts/<agent>/`). Markdown sources gain archive frontmatter (storage time, source URL, content hash, which wiki pages they produced); binaries get a `.meta.yaml` sidecar. Read-only history. |
| `_raw/` | Drop new sources here; `wiki-ingest` promotes them and **archives the originals to `source/`** (never deletes) |
| `_staging/` | Review queue when `WIKI_STAGED_WRITES=true` |
| `_archives/` | Snapshots from rebuild/restore operations *(distinct from `source/` — wiki snapshots vs. raw inputs)* |
| `index.md` | Auto-maintained catalog of every wiki page |
| `log.md` | Append-only chronological record |
| `hot.md` | ~500-word semantic snapshot of recent activity |
| `.manifest.json` | Bookkeeping: which sources have been ingested, with content hashes |
| `.obsidian/` | Obsidian app config (shared) |
| `.claude/skills/` | Skill definitions Claude Code loads |
| `.hermes/skills/` | Skill definitions Hermes loads |
| `skills-lock.json` | Pinned skill versions sourced from `Ar9av/obsidian-wiki` |
| `.env` | Vault configuration (paths, history sources, thresholds) |

---

## Getting started

### 0. (Only if starting a fresh vault) Install the framework

This repo is already wired up — skip to step 1. If you want to bootstrap a *new* vault from scratch, the upstream offers three installation paths:

```bash
# Option A — pip (recommended for end users)
pip install obsidian-wiki
obsidian-wiki setup --vault /path/to/vault

# Option B — Skills CLI (drop skills into an existing project)
npx skills add Ar9av/obsidian-wiki

# Option C — Git clone (track upstream changes directly)
git clone https://github.com/Ar9av/obsidian-wiki
cd obsidian-wiki && bash setup.sh
```

After installation, run `/wiki-setup` inside your AI agent to scaffold the vault structure (categories, special files, `.env`).

### 1. Open the vault in Obsidian

`File → Open Vault → ` *(this repository's root directory)*

### 2. (Optional) Install community plugins

Recommended for the full experience:

- **Dataview** — query frontmatter, build dynamic tables. Essential at scale.
- **Graph Analysis** — richer graph view metrics on top of the built-in graph
- **Templater** — manual page templates if you ever author directly
- **Obsidian Git** — auto-backup the vault to a git repo
- **Web Clipper** — browser → markdown into `_raw/`; the fastest way to feed sources in

### 3. Ingest your first sources

Inside Claude Code, run:

```
/wiki-status     # see what's pending
/wiki-ingest     # distill source documents into pages
```

To mine past AI conversations:

```
/claude-history-ingest    # ~/.claude sessions
/codex-history-ingest     # ~/.codex sessions
/hermes-history-ingest    # ~/.hermes sessions
```

### 4. Ask questions and let answers compound

```
/wiki-query           # answer with citations from the vault
/wiki-research <X>    # autonomous multi-round web research, filed into the wiki
/wiki-synthesize      # find concepts that co-occur and need a synthesis page
```

### 5. Keep the graph healthy

```
/wiki-lint             # orphans, broken links, contradictions
/cross-linker          # add missing wikilinks
/wiki-status --insights # hubs, bridges, fragmented tag clusters
/daily-update          # cron-friendly daily refresh
```

---

## Agent compatibility

The upstream framework ships skill bundles for many AI coding agents — they all read the same vault format. **MeteorDreamWiki currently has skill files committed for three of them** (`.claude/`, `.hermes/`, `.agents/`), but you can drop in any of the others as needed.

| Agent | Skill discovery path | Status in this repo |
|---|---|---|
| Claude Code | `.claude/skills/` | ✅ committed |
| Hermes | `.hermes/skills/` | ✅ committed |
| Cursor / Windsurf / generic | `.agents/skills/` | ✅ committed |
| Codex | `~/.codex` (history mining only) | history path configured |
| GitHub Copilot CLI | `~/.copilot/` | use `/copilot-history-ingest` to mine |
| Pi (OpenCode) | `~/.pi/agent/sessions/` | use `/pi-history-ingest` to mine |
| OpenClaw | `~/.openclaw/` | use `/openclaw-history-ingest` to mine |
| Gemini CLI / Kiro | per-agent paths | not yet wired in this vault |

The vault content (every `.md` page, the index, the log, the manifest) is **plain markdown** — none of it is agent-specific. Switching agents only changes which skill files load.

---

## Configuration (`.env`)

| Variable | Purpose |
|---|---|
| `OBSIDIAN_VAULT_PATH` | Vault root. Use `.` to mean "this repo." |
| `OBSIDIAN_SOURCES_DIR` | Comma-separated list of dirs `wiki-ingest` scans. **Default `_raw`** — drop sources there. Don't set to `.` unless you want skill files treated as sources. |
| `CLAUDE_HISTORY_PATH` | `~/.claude` — enables `claude-history-ingest` |
| `CODEX_HISTORY_PATH` | `~/.codex` — enables `codex-history-ingest` |
| `HERMES_HISTORY_PATH` | `~/.hermes` — enables `hermes-history-ingest` |
| `WIKI_TOKEN_WARN_THRESHOLD` | Warn when a full-wiki read would exceed N tokens (default `100000`, `0` to disable) |
| `WIKI_STAGED_WRITES` | `true` routes all LLM-written pages through `_staging/` for review |
| `QMD_*` | Optional — enables semantic search via the [QMD](https://github.com/tobi/qmd) backend; falls back to grep otherwise |

---

## Skill catalog

The `.claude/skills/` folder bundles ~38 skills. Each one is a refinement of Ingest, Query, or Lint.

### Ingestion
- **`wiki-ingest`** — distill documents from `OBSIDIAN_SOURCES_DIR` into wiki pages
- **`data-ingest`** — catch-all for raw text dumps, ChatGPT exports, transcripts
- **`ingest-url`** — fetch a URL and add it as a wiki page
- **`claude-history-ingest`** / **`codex-history-ingest`** / **`hermes-history-ingest`** / **`pi-history-ingest`** / **`copilot-history-ingest`** / **`openclaw-history-ingest`** — agent-specific transcript miners
- **`wiki-history-ingest`** — unified router for all of the above
- **`wiki-quick-chat-capture`** — fast 60-second capture of the current conversation to `_raw/`
- **`wiki-capture`** — turn the current conversation into a permanent structured page

### Querying & exploration
- **`wiki-query`** — answer questions over the vault with citations
- **`wiki-agent`** — query a *specific* agent's history (e.g. "what did I do in Codex about X")
- **`memory-bridge`** — diff knowledge across AI tools to surface blind spots
- **`wiki-context-pack`** — token-bounded context slice for downstream agents
- **`wiki-research`** — autonomous multi-round web research filed into the wiki

### Maintenance (the "Lint" operation)
- **`wiki-status`** — pending ingestion, token footprint, hub/bridge insights
- **`wiki-lint`** — orphan pages, broken wikilinks, contradictions; `--consolidate` to act-and-report
- **`wiki-dedup`** — merge pages covering the same concept under different names
- **`cross-linker`** — discover and write missing wikilinks
- **`wiki-synthesize`** — propose new synthesis pages from co-occurrence patterns
- **`tag-taxonomy`** — enforce a controlled tag vocabulary
- **`daily-update`** — daily refresh cycle (cron-friendly)
- **`wiki-digest`** — human-readable weekly/monthly knowledge digest

### Lifecycle
- **`wiki-setup`** — initial vault bootstrap (already run for this repo)
- **`wiki-rebuild`** — archive and rebuild from sources
- **`wiki-export`** / **`wiki-import`** — graph.json, GraphML, Cypher, interactive HTML
- **`wiki-stage-commit`** — review/promote staged pages when `WIKI_STAGED_WRITES=true`
- **`wiki-switch`** — switch between named vault profiles
- **`wiki-update`** — sync knowledge from any project into this vault

Run `/<skill-name>` inside Claude Code to invoke any of these.

### Two portable "global" skills

Two skills are designed to work **across projects**, not just inside this vault — they're the bridge between any project you're working in and your knowledge base:

- **`/wiki-update`** — *push.* From any project directory, distill what you've been working on into the vault. The skill figures out which project bucket the knowledge belongs in.
- **`/wiki-query`** — *pull.* From any project directory, ask a question and get a cited answer assembled from the wiki. No need to be inside the vault.

Together they let you treat the vault as a long-term memory store that you can read from and write to no matter which project you're currently in.

---

## Conventions

- **You curate, the LLM writes.** Don't hand-edit pages unless you have to — let the skills maintain consistency.
- **Frontmatter is required.** Every page has `title`, `tags`, `summary`, `sources`, `base_confidence`, `lifecycle`, `tier`, `provenance`. See `tag-taxonomy`.
- **Wikilinks over plain prose.** `[[Concept Name]]` is what builds the graph.
- **Provenance markers per claim.** `^[extracted]` (stated by source), `^[inferred]` (synthesized), `^[ambiguous]` (sources disagree). The wiki's value depends on the user being able to tell signal from synthesis.
- **`_raw/` is a transient inbox; `source/` is permanent.** `wiki-ingest` reads from `_raw/`, distills into wiki pages, then **moves the original to `source/<topic>/`** (with archive frontmatter for `.md` files, sidecar `.meta.yaml` for binaries) — never deletes. The provenance anchor stays in the vault forever.
- **`hot.md` is rewritten frequently.** Don't hand-edit; let the skills maintain it.
- **The wiki is a git repo.** You get version history, branching, and collaboration for free.

---

## Why this is worth the setup

Most personal knowledge bases die because the bookkeeping outpaces the value. This one doesn't, because the bookkeeper is an LLM. **What you accumulate is a graph, not a folder of files** — and the graph gets richer with every source you add and every question you ask.

The wiki is yours forever. The agents come and go.

---

## License

This project is licensed under the **[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/)** license — see [LICENSE](./LICENSE) for the full text.

In short: you can share and adapt the material, with attribution, for non-commercial purposes, and any derivative work must use the same license.

The skill collection under `.claude/skills/`, `.hermes/skills/`, and `.agents/skills/` is sourced from [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki) and is governed by that repository's license — see `skills-lock.json` for the pinned versions.

The LLM Wiki methodology is articulated by Andrej Karpathy in the [original essay](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), ingested into this vault as [[references/karpathy-llm-wiki-essay]]. The pattern echoes [Vannevar Bush's 1945 Memex](https://en.wikipedia.org/wiki/Memex) ([[references/memex]]).
