---
title: "As We May Think (Memex, 1945)"
category: references
tags:
  - reference
  - history-of-computing
  - hypertext
  - knowledge-management
summary: Vannevar Bush's 1945 essay describing the Memex — a hypothetical desk-sized device for storing and associatively linking documents. Anticipated hypertext and the LLM Wiki pattern; the maintenance problem was its unsolved part.
sources:
  - "agent:wiki-ingest karpathy_llm_wiki.md (raw)"
created: 2026-06-06T10:16:21Z
updated: 2026-06-06T10:16:21Z
base_confidence: 0.4
lifecycle: draft
lifecycle_changed: 2026-06-06
tier: peripheral
provenance:
  extracted: 0.30
  inferred: 0.55
  ambiguous: 0.15
relationships:
  - target: "[[entities/vannevar-bush]]"
    type: related_to
  - target: "[[concepts/llm-wiki-pattern]]"
    type: related_to
---

# As We May Think (Memex, 1945)

The 1945 essay by [[entities/vannevar-bush]], published in *The Atlantic*, that introduced the **Memex** — a hypothetical mechanical desk for storing documents on microfilm and linking them together with associative trails between any two pages.

## Why this reference is in the wiki

Karpathy explicitly traces the [[concepts/llm-wiki-pattern]] back to the Memex:

> The idea is related in spirit to Vannevar Bush's Memex (1945) — a personal, curated knowledge store with associative trails between documents. Bush's vision was closer to this than to what the web became: private, actively curated, with the connections between documents as valuable as the documents themselves. The part he couldn't solve was who does the maintenance. The LLM handles that. ^[extracted]

So this reference exists to anchor the **lineage**: Bush identified the form (private, curated, associative), the LLM Wiki pattern adds the missing actor (the LLM as maintainer).

## What the Memex proposed (high level)

- A personal device, not a public network
- Documents stored locally on microfilm
- The user (or a "trail-blazer") could create persistent associations between any two documents
- Trails could be saved, shared, and inherited

What it did **not** propose:
- Public hyperlinks (the web did that)
- Automatic maintenance (the LLM Wiki pattern does that)
- Search beyond traversal (not relevant in 1945)

## Open / unsourced

Specific quotes from the essay, the publication context (post-WWII, *The Atlantic*, July 1945), and the influence chain (Memex → Engelbart's NLS → Nelson's Project Xanadu → the WWW) are widely documented but **not yet ingested into this vault**. Future references on these would strengthen the page. ^[ambiguous]

## See also

- [[entities/vannevar-bush]]
- [[concepts/llm-wiki-pattern]]
- [[concepts/persistent-knowledge-graph]]
