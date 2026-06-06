# MeteorDreamWiki

An [Obsidian](https://obsidian.md) vault wired into the [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki) skill collection — a knowledge base where Claude Code (and other AI agents) ingest your notes, conversations, and source documents and distill them into interconnected wiki pages.

The repository root *is* the vault. Open this folder in Obsidian and you're done.

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
| `_raw/` | Rough drafts staged for promotion by `wiki-ingest` |
| `_staging/` | Review queue when `WIKI_STAGED_WRITES=true` |
| `_archives/` | Snapshots from rebuild/restore operations |
| `index.md` | Auto-maintained wiki index |
| `log.md` | Append-only operation log |
| `hot.md` | ~500-word semantic snapshot of recent activity |
| `.obsidian/` | Obsidian app config |
| `.claude/skills/` | The skill definitions Claude Code loads |
| `skills-lock.json` | Pinned skill versions sourced from `Ar9av/obsidian-wiki` |
| `.env` | Vault configuration (paths, history sources, thresholds) |

---

## Getting started

### 1. Open the vault in Obsidian

`File → Open Vault → ` *(this repository's root directory)*

### 2. (Optional) Install community plugins

Recommended for the full experience:

- **Dataview** — query metadata, build dynamic tables
- **Graph Analysis** — richer graph view
- **Templater** — manual page templates
- **Obsidian Git** — auto-backup the vault to git

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

---

## Configuration (`.env`)

| Variable | Purpose |
|---|---|
| `OBSIDIAN_VAULT_PATH` | Vault root (this repo) |
| `OBSIDIAN_SOURCES_DIR` | Comma-separated list of source directories that `wiki-ingest` scans |
| `CLAUDE_HISTORY_PATH` | `~/.claude` — enables `claude-history-ingest` |
| `CODEX_HISTORY_PATH` | `~/.codex` — enables `codex-history-ingest` |
| `HERMES_HISTORY_PATH` | `~/.hermes` — enables `hermes-history-ingest` |
| `WIKI_TOKEN_WARN_THRESHOLD` | Warn when a full-wiki read would exceed N tokens (default `100000`, `0` to disable) |
| `WIKI_STAGED_WRITES` | `true` routes all LLM-written pages through `_staging/` for human review |
| `QMD_*` | Optional — enables semantic search via the [QMD](https://github.com/Ar9av/qmd) backend; falls back to grep otherwise |

---

## Core skills

The `.claude/skills/` folder bundles 38 skills. The ones you'll touch most:

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

### Maintenance
- **`wiki-status`** — pending ingestion, footprint, central hubs
- **`wiki-lint`** — orphan pages, broken wikilinks, contradictions; `--consolidate` to act-and-report
- **`wiki-dedup`** — merge pages covering the same concept
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
- **`wiki-update`** — sync the current project's knowledge into the vault from anywhere

Run `/<skill-name>` inside Claude Code to invoke any of these.

---

## Architecture

The vault follows the three-layer pattern from the `llm-wiki` skill (inspired by Karpathy's LLM Wiki):

```
sources              wiki                 schema
────────────         ─────────            ─────────
your docs    ──►    distilled       ──►   tags, links,
agent logs          interconnected         frontmatter,
URLs, code          markdown pages         dataview queries
```

Sources are read-only inputs. The wiki is the curated, deduplicated layer. The schema (frontmatter + wikilinks + tag taxonomy) is what makes it queryable as a graph.

---

## Conventions

- **No emojis in pages** unless explicitly requested
- **Frontmatter required** on every page (`title`, `tags`, `lifecycle`, etc. — see `tag-taxonomy`)
- **Wikilinks over plain prose** — `[[Concept Name]]` builds the graph
- **`_raw/` is disposable** — `wiki-ingest` promotes drafts and deletes the originals
- **`hot.md` is rewritten frequently** — don't hand-edit; let the skills maintain it

---

## License

The skill collection is sourced from [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki); see that repository for licensing of the skill code. Vault content (your notes) is yours.
