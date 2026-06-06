---
title: kepano/obsidian-skills
category: entities
tags:
  - tool
  - skill-pack
  - obsidian
  - knowledge-management
url: https://github.com/kepano/obsidian-skills
summary: A companion skill pack by Steph Ango (kepano) that teaches AI agents Obsidian-format mastery — wikilinks, callouts, Bases, JSON Canvas, defuddle. Designed to coexist with obsidian-wiki under the same Agent Skills spec.
sources:
  - "https://github.com/Ar9av/obsidian-wiki/blob/main/README.md"
created: 2026-06-06T13:29:22Z
updated: 2026-06-06T13:29:22Z
base_confidence: 0.5
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: peripheral
provenance:
  extracted: 0.55
  inferred: 0.30
  ambiguous: 0.15
relationships:
  - target: "[[entities/obsidian-wiki-framework]]"
    type: related_to
  - target: "[[entities/obsidian]]"
    type: uses
---

# kepano/obsidian-skills

A skill pack designed to be installed **alongside** [[entities/obsidian-wiki-framework|`Ar9av/obsidian-wiki`]]. The split of responsibilities is intentional: `obsidian-wiki` handles the *knowledge-management workflow* (ingest, query, lint, rebuild); `obsidian-skills` handles *Obsidian format mastery*.

**Repository:** <https://github.com/kepano/obsidian-skills>
**Author:** Steph Ango (kepano) — also CEO of Obsidian, so first-party authority on the format ^[inferred]

## What it adds (per the upstream README)

| Skill | What it adds |
|---|---|
| `obsidian-markdown` | Teaches the agent correct Obsidian-flavored syntax — wikilinks, callouts, embeds, properties |
| `obsidian-bases` | Create and edit `.base` files (database-like views of notes) |
| `json-canvas` | Create and edit `.canvas` files (visual mind maps, flowcharts) |
| `obsidian-cli` | Interact with a running Obsidian instance via CLI (search, create, manage notes) |
| `defuddle` | Extract clean markdown from web pages — less noise than raw fetch, saves tokens during ingest |

`defuddle` is particularly worth knowing about: it's an LLM-economy optimization. When `/ingest-url` or web-clipper-fed `_raw/` files contain heavy boilerplate (nav, ads, footers), `defuddle` strips it before the LLM has to read it. ^[inferred]

## Why they coexist cleanly

> Both projects use the same [Agent Skills spec](https://agentskills.io/specification), so they coexist in the same `.skills/` directory with no conflicts. ^[extracted]

This is the value of the spec: any skill bundle following it can be dropped into any compliant agent's discovery path and work alongside any other compliant skill bundle. ^[inferred]

## Install

```bash
npx skills add kepano/obsidian-skills
```

## Status in MeteorDreamWiki

Not currently installed. The current vault doesn't author `.canvas` or `.base` files yet, and `wiki-ingest` already handles its own URL fetching. Adding this would be most useful if:

- You want the agent to start producing `.canvas` mind maps from synthesis pages
- Your `_raw/` will receive raw web HTML (not pre-cleaned markdown) and you want `defuddle` to strip boilerplate ^[inferred]

## Open questions

- Whether kepano's skills assume a specific agent or work cross-agent like obsidian-wiki ^[ambiguous]
- Whether they have their own delta tracking or rely on agent-level state ^[ambiguous]
- Versioning / release cadence ^[ambiguous]

## See also

- [[entities/obsidian-wiki-framework]]
- [[entities/obsidian]]
- [[references/obsidian-wiki-readme]]
