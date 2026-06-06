---
title: Hot Cache
updated: 2026-06-06T13:09:10Z
---

# Hot Cache

*A ~500-word semantic snapshot of recent activity. Updated after every major write operation.*

## Recent Activity

- **[2026-06-06T13:09:10Z] CONVENTION_CHANGE** — Introduced the **archive-don't-delete** rule. `wiki-ingest` no longer deletes promoted `_raw/` files; instead it moves them to `source/<topic>/<filename>` (creating the directory) and adds an archive frontmatter block to `.md` sources (storage time, source URL, content hash, the wiki pages it produced) or a `.meta.yaml` sidecar to binaries. Updated `wiki-ingest` and `wiki-setup` skill files in all three trees (`.claude/.hermes/.agents`); refreshed `README.md`, `README-zh.md`, [[references/karpathy-llm-wiki-essay]] local-provenance section, and `.manifest.json`. Bootstrapped `source/llm/knowledge/karpathy_llm_wiki.md` retroactively.
- **[2026-06-06T12:53:59Z] STAGE_COMMIT** — Promoted all 4 staged files. `entities/obsidian-wiki-framework` is now live; three patches applied to llm-wiki-pattern, wiki-operations-trinity, obsidian.
- **[2026-06-06T12:44:17Z] INGEST (staged)** — Distilled the upstream `Ar9av/obsidian-wiki` README into project READMEs and a staged framework page.

## Active Threads

- **`source/` is now a tracked first-class directory.** It will fill up on every future `/wiki-ingest` run from `_raw/`. The first entry — `source/llm/knowledge/karpathy_llm_wiki.md` — is in place as a working example of the archive frontmatter schema.
- **Skill drift across trees.** `.claude/.hermes/.agents` skill bundles still in lockstep — both updated skills (`wiki-ingest`, `wiki-setup`) verified by SHA-256. `skills-lock.json`'s `computedHash` for these two will now diverge from upstream; that's expected and intentional. If a future drift checker complains, point it at `log.md`'s `CONVENTION_CHANGE` entry.
- **The 2-byte hash divergence.** When recovering Karpathy's essay into `source/`, the Write tool added 2 trailing newline bytes. Original `content_hash` is recorded for ingest skip-detection; `archived_content_hash` records what's actually on disk now. The convention is documented in the skill so future archives handle this consistently.

## Key Takeaways

- **Provenance never gets thrown away.** Every byte of every raw input that ever produced a wiki page now lives in `source/<topic>/` forever, with frontmatter (or sidecar) metadata explaining where it came from and which wiki pages it produced.
- **`_raw/` and `source/` have distinct semantics.** `_raw/` = transient inbox (everything in it is unprocessed); `source/` = permanent archive (everything in it has been processed and is read-only history).
- **The archive frontmatter schema** for `.md` sources includes: `title`, `source_url`, `source_author`, `source_kind`, `source_format`, `license`, `archived_at`, `archived_by`, `original_raw_path`, `size_bytes`, `content_hash`, `ingested_into[]`, optional `notes`, plus `archived_content_hash` when the as-archived bytes diverge.
- **Manifest entries gain three fields:** `original_raw_path`, `archived_path`, `archived_at`. Subsequent ingest runs use `content_hash` to skip duplicates regardless of where the file lives.

## Flagged Contradictions

*None yet.*
