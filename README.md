# Agent-Wiki-Idea

A planning / notes wiki exploring what a wiki might look like if its **primary users are AI agents rather than humans**.

## Premise

Traditional wikis (MediaWiki, DokuWiki, Notion, Confluence, even most "knowledge bases") are designed around a human reader: hierarchical navigation, page titles, sidebars, breadcrumbs, hyperlinks between articles, search-by-keyword. The unit of consumption is "a page a person reads."

We are now in a world where:

- A growing share of reads against documentation and knowledge bases come from LLM agents, not humans.
- Tools and capabilities are increasingly exposed via MCP, function-calling, and agent runtimes.
- Retrieval is moving from keyword/BM25 to dense vector / hybrid semantic search.
- Agents don't need a "page"; they need the smallest self-contained chunk that answers a question, with metadata about provenance and freshness.

So: **what would a wiki look like if you designed it knowing the median reader is an agent?**

## Working hypotheses

1. **Storage is vector-native, not page-native.** The atomic unit is a chunk (claim, fact, procedure, decision) with embeddings, not a wiki page. Pages, if they exist at all, are views over chunks.
2. **Writes happen via skills/tools, not editors.** Agents (and humans, via agents) `upsert` new knowledge through a small set of capability tools — `add_fact`, `add_procedure`, `update_status`, `deprecate` — rather than editing markdown.
3. **Reads happen via semantic search, not navigation.** No sidebar, no tree. The only read interface is `search(query, filters)` returning ranked chunks with citations.
4. **Provenance is first-class.** Every chunk carries author, timestamp, source, confidence, and supersession links — agents need to know how much to trust a result.
5. **Freshness is enforced.** Chunks have TTLs or review dates; stale entries are demoted or auto-flagged for re-verification.
6. **Human-readable views are generated, not authored.** If a human wants to "browse," a renderer composes a page on demand from the underlying chunks.
7. **Schemas are loose but typed.** Chunks declare a type (fact, procedure, decision, glossary, contact, …) so that retrieval and rendering can specialize.

## Open questions to explore in `notes/`

- Chunk schema: what metadata is mandatory vs optional?
- Conflict resolution: two agents upsert contradictory facts — what wins?
- Identity & trust: do chunks need signed authorship?
- Deduplication: semantic dedup at write time vs read time.
- The "page" question: is the page concept fully dead, or does it return as a cached projection?
- Migration: how would you fold an existing human wiki (MediaWiki dump, Notion export) into this model?
- Read shape: is pure semantic search enough, or do agents also need structured queries (filter by type/tag/date)?
- Governance: who can deprecate? Who can mark a chunk authoritative?
- Eval: how do you measure whether the wiki is "good" if no human reads it?

## Status

Pure thinking repo. No code yet. `notes/` is the scratchpad.
