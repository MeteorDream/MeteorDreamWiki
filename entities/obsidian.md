---
title: Obsidian
category: entities
tags:
  - tool
  - markdown
  - knowledge-management
  - wiki
summary: A local-first markdown editor with wikilinks, graph view, and a plugin ecosystem — the canonical viewer/editor for an LLM-maintained wiki and the tool this vault is configured for.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.55
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: supporting
provenance:
  extracted: 0.65
  inferred: 0.30
  ambiguous: 0.05
relationships:
  - target: "[[concepts/llm-wiki-pattern]]"
    type: uses
  - target: "[[concepts/persistent-knowledge-graph]]"
    type: implements
---

# Obsidian

A local-first markdown editor that operates over a "vault" — a directory of `.md` files plus a `.obsidian/` config folder. It treats `[[wikilinks]]` as first-class citizens and renders the resulting page graph visually.

This vault is an Obsidian vault. The repository root is the vault root; opening this repo as a vault in Obsidian yields the full wiki experience.

## Role in the LLM Wiki pattern

In Karpathy's framing: **Obsidian is the IDE.** ^[extracted] The LLM writes the pages; Obsidian is what you use to read, browse, and follow links.

The features that make it well-suited:

- **Wikilink resolution.** `[[Page Name]]` resolves to the file with that title — no path management needed. ^[extracted]
- **Graph view.** Visual representation of the page graph; immediate sense of [[concepts/persistent-knowledge-graph|hub structure]]. ^[extracted]
- **Live preview.** Renders markdown without leaving edit mode.
- **Plugin ecosystem** — Dataview (frontmatter queries), Templater (page templates), Marp (slides from markdown), Web Clipper (browser → markdown). ^[extracted]

## Recommended plugins (for this pattern)

From the source essay and this vault's `wiki-setup` skill:

- **Dataview** — query frontmatter, build dynamic tables. Essential at scale. ^[extracted]
- **Graph Analysis** — richer structural metrics on top of the built-in graph view.
- **Templater** — manual page templates for when the human authors directly.
- **Obsidian Git** — auto-backup the vault to a git repo (this vault is already a git repo, so this is more useful for the `_staging/` workflow). ^[inferred]
- **Web Clipper** — browser extension that converts pages to markdown; fast path for getting sources into `_raw/`. ^[extracted]

## Image handling tip

From the source essay: set `Settings → Files and links → Attachment folder path` to a fixed directory (e.g. `raw/assets/`). Bind a hotkey to "Download attachments for current file" to pull all referenced images locally — useful because LLMs can't natively read markdown with inline images in one pass; they have to view images separately. ^[extracted]

## Why local-first matters

The vault is just a directory. No vendor lock-in, no cloud sync required, no proprietary format. You can:

- Version with git (this repo does)
- Back up with rsync
- Search with grep, [[entities/qmd]], or any other tool
- Move to a different editor without converting anything

## See also

- [[concepts/llm-wiki-pattern]]
- [[concepts/persistent-knowledge-graph]]
- [[entities/qmd]]
- [[skills/index-and-log-files]]
