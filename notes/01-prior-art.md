# Prior Art: Agent-First Wikis and Knowledge MCPs

## Scope

Mapping prior art for an "agent-first wiki" — a knowledge store whose primary readers and writers are LLM agents over MCP, not humans browsing pages. Inclusion criteria:

1. Knowledge stores designed agent-primary (not human-primary with API).
2. MCP servers whose core service is exposing/curating a knowledge base.
3. Adjacent: vector-native note stores with agent write APIs, "memory servers", and `llms.txt` / `agents.md`-style standards making sites agent-readable.

Use case driving the search: home/homelab knowledge — network maps, SOPs, skills/procedures — written and read by agents over MCP, ideally in token-efficient formats (e.g. TOON). Same shape generalizes to corporate. Centralized server, with auth/federation acknowledged as unsolved. Pain point being avoided: maintaining a human wiki AND a parallel vectorized copy.

---

## Agent memory / knowledge MCP servers

| Project | Link | What it is | Verdict vs "agent-first wiki" |
|---|---|---|---|
| **Basic Memory** | [basicmachines-co/basic-memory](https://github.com/basicmachines-co/basic-memory) | Local Markdown-with-wiki-links knowledge base. LLMs read/write via MCP tools; Obsidian opens the same files natively. | **Strong fit.** Closest existing match: agents are first-class writers, files are plain Markdown so a human can dip in, no parallel vector copy required. Lacks server/auth story. |
| **Mem0 MCP** | [mem0.ai](https://mem0.ai) / [mem0ai/mem0-mcp](https://github.com/mem0ai/mem0-mcp) | Hosted/self-hostable agent memory layer; MCP server exposes add/search. | Partial. Memory shape (chat-derived facts) not wiki-shape (curated SOPs/maps). Good plumbing, wrong content model. |
| **Zep + Graphiti MCP** | [getzep/graphiti](https://github.com/getzep/graphiti), [Zep KG MCP](https://www.getzep.com/product/knowledge-graph-mcp/) | Temporal knowledge graph with bitemporal edges; MCP server for Claude/Cursor. | Partial. Stronger than Mem0 for structured facts; still graph-of-events, not wiki-of-pages. Useful as the "evolving truth" layer behind a wiki. |
| **Cognee** | [cognee.ai](https://www.cognee.ai/) | GraphRAG over multi-doc corpora; MCP exposed. | Partial. Document-first, ingest-heavy. Closer to a RAG index than a wiki authors edit. |
| **Letta (ex-MemGPT)** | [letta.com](https://www.letta.com/) | Agent runtime with OS-style memory tiers. | Tangential. Memory is per-agent state, not a shared wiki. |
| **Supermemory** | [supermemory.ai](https://supermemory.ai/) | "Second brain" memory product, MCP and llm-bridge. | Partial. Personal-memory framing; agent-readable but not wiki-structured. |
| **Graphiti (standalone)** | [getzep/graphiti](https://github.com/getzep/graphiti) | Real-time temporal KG engine. | Partial. Engine, not a wiki. Pair with editor + MCP and you'd have something. |
| **MCP Knowledge Graph (reference)** | [modelcontextprotocol/servers/memory](https://github.com/modelcontextprotocol/servers/tree/main/src/memory) | Reference "memory" server: entities + relations in a JSON file. | Tangential. Demo-grade, but proves the agent-write pattern. |
| **sage-wiki** | [toolhunter listing](https://toolhunter.cc/tools/sage-wiki) | Go CLI that compiles folders of mixed docs into an interlinked wiki; runtime exposes CLI + MCP + optional browser UI. | **Strong fit on framing** ("MCP face, not bolted on"). Worth a closer look — verify project still active before relying on it. |
| **llm-wiki-agent** | [SamurAIGPT/llm-wiki-agent](https://github.com/SamurAIGPT/llm-wiki-agent) | Drop-in sources, agent extracts and maintains an interlinked wiki. Works with Claude Code, Codex, Gemini, OpenCode. | **Strong fit.** Agent-as-author wiki, no API key required, runs against any coding-agent CLI. Closest to the "skills write to wiki" idea. |
| **mcp-knowledge-vault** | [StuMason/mcp-knowledge-vault](https://github.com/StuMason/mcp-knowledge-vault) | Structured store for agent-managed info via MCP. | Partial. Smaller, less proven. |
| **mcp-brain** | [TouristShaun/mcp-brain](https://github.com/TouristShaun/mcp-brain) | Persistent KG on Supabase, shared across Claude installs. | Partial. Multi-client sharing is the right instinct; KG-shape, not wiki-shape. |
| **memory-graph** | [memory-graph/memory-graph](https://github.com/memory-graph/memory-graph) | Graph-DB MCP memory for coding agents. | Tangential. Code-agent specific. |
| **Cloudflare AutoRAG (+ MCP)** | [blog](https://blog.cloudflare.com/introducing-autorag-on-cloudflare/), [MCP](https://www.pulsemcp.com/servers/cloudflare-autorag) | Managed RAG pipeline (R2 + Vectorize + Workers AI), MCP endpoint for agents. Hybrid (BM25 + vector). | Partial. Read-side excellent, write-side is "drop files in R2 and reindex" — not "agent edits a page." Centralized + authenticated, which is the federation angle. |

---

## llms.txt / agents.md and agent-readable doc standards

[`llms.txt`](https://llmstxt.org/) is a `/llms.txt` markdown index plus parallel `*.md` mirrors of HTML pages, so an LLM can find and ingest content without rendering or scraping. Adopted by Anthropic, Cursor, Stripe, ElevenLabs, Hugging Face, Cloudflare, Mintlify customers. [Mintlify's content-negotiation extension](https://www.mintlify.com/blog/context-for-agents) serves the `.md` variant when an agent UA is detected.

[Cloudflare "Docs for agents"](https://developers.cloudflare.com/docs-for-agents/) is the same idea operationalized: every doc page has an agent-readable twin.

**What these solve:** read-side discoverability — making an existing human site/docs ingestible by an agent without HTML pain.

**What these don't solve:**
- Write side. There is no "agent commits a new SOP" semantics in `llms.txt`. It's strictly publish-time output.
- Curation. The index is whatever you put in it; no skill says "add this page when you learn X."
- Schema. Pages are still prose-shaped, not structured (TOON, frontmatter graphs, etc.).

For a centralized homelab wiki, `llms.txt` is interesting as the **export format**, not the source of truth.

[NLWeb](https://github.com/microsoft/NLWeb) (Guha / Microsoft) is adjacent: `/ask` (REST) and `/mcp` (agent endpoint) on top of Schema.org-annotated content. Two-faced site: humans + agents querying the same data. Read-only, again — write story isn't the focus.

`agents.md` exists as a convention some repos adopt (instructions for coding agents, parallel to `llms.txt`) but is not a ratified standard and doesn't apply to wiki content.

---

## Vector-native note / knowledge stores

These are the candidates that come closest to "the wiki IS the agent's view, no parallel vector copy":

- **Basic Memory** — files-on-disk, MCP-front, Obsidian as optional human reader. The cleanest answer to the "no duplication" pain point. Auth is "filesystem permissions"; no remote/multi-user story.
- **Graphiti** — facts-as-graph with temporal edges. Different shape (no "page"), but the "agents append, system reconciles" pattern is right.
- **Cognee** — GraphRAG, ingest-heavy. Closer to corporate KB than homelab wiki.
- **Mem0** — chat-residue memory; not the right shape.
- **Letta** — per-agent memory; not a shared store.
- **Supermemory / OpenMemory / SuperLocalMemory** — personal-memory products with MCP. Right plumbing, "personal" framing instead of "shared wiki."
- **Cloudflare AutoRAG** — R2-backed RAG with MCP. Solid read side and the only one in the list with a credible auth/multi-tenant story out of the box, but write semantics are "upload a file."

---

## Closest matches to Daniel's framing

1. **[Basic Memory](https://github.com/basicmachines-co/basic-memory)** — files = source of truth, MCP is the primary write/read API, humans optional via Obsidian. The "no parallel vector copy" pain is solved by design (it doesn't run a vector index by default; relies on link-graph traversal). Gap: single-user, local-only.
2. **[llm-wiki-agent](https://github.com/SamurAIGPT/llm-wiki-agent)** — explicit "agent maintains the wiki" framing. Works across Claude Code / Codex / Gemini CLI. Gap: still a local repo of Markdown; no centralized server, no auth, no skill-shaped write API beyond what the host agent provides.
3. **[Cloudflare AutoRAG](https://blog.cloudflare.com/introducing-autorag-on-cloudflare/) + NLWeb** — closest credible centralized + authenticated + agent-readable stack. Gap: read-side; write semantics are "file upload, reindex," not "agent runs a `wiki:add-sop` skill."

---

## Gaps — what nobody seems to have built

The combination Daniel is describing isn't sitting on a shelf:

- **Home/SOP-shaped content model.** Memory MCPs target chat-residue ("user likes dark mode"); KG MCPs target temporal facts; doc MCPs target prose pages. Nobody is shipping a server whose primary noun is "SOP" / "device" / "network segment" / "skill writeup" with a schema that fits homelab/runbook content.
- **MCP-primary, human-optional.** Basic Memory is the only one that genuinely qualifies. Everything else is either "human wiki + MCP plugin" (excluded by criteria) or "memory blob the human can't really browse."
- **Centralized server with auth/federation.** Local-first dominates: Basic Memory, llm-wiki-agent, Graphiti self-host. The hosted ones (Mem0, Zep, Supermemory, AutoRAG) are SaaS-shaped (account = tenant) and not designed for "my homelab agent + my work agent both write to my personal wiki under different scopes." OAuth-per-skill against a wiki namespace doesn't exist as a product.
- **Write-via-skill ergonomics.** Agents writing prose blobs is a solved problem. Agents writing *structured*, *schema-validated*, *idempotent* entries (e.g. `wiki:upsert-device(host=zima, role=nas, …)`) — the kind of write that doesn't drift into duplicates over 1000 calls — is not. Closest is Graphiti's entity dedup, which is graph-shaped, not page-shaped.
- **Token-efficient native format.** No agent wiki ships TOON or a similar dense format as a first-class read shape. `llms.txt` is Markdown; memory MCPs return JSON. A wiki that serves TOON to agents and Markdown to humans from the same source is open territory.
- **Single-source-of-truth guarantee.** Every stack that adds vectors adds a parallel copy. Basic Memory's link-graph approach is the only "no duplicate index" answer found, and it pays for it in retrieval quality.

The shape of the missing thing: a self-hostable server, centralized, MCP-primary, auth-aware, with a structured-page schema (network/device/SOP/skill), TOON-on-the-wire to agents, Markdown export to humans, schema-validated agent writes, and either no vector index or a derived-and-disposable one.

---

## Explicitly out of scope

Excluded under the "human wiki with MCP bolted on" rule. Considered and rejected:

- **MediaWiki** — human wiki; MCP plugins exist but the wiki is for humans first.
- **Notion** — human SaaS wiki; official + community MCPs are read/write wrappers around a UI-driven product.
- **Confluence** — enterprise human wiki; Atlassian MCP wraps the human API.
- **Outline** — human team wiki; community MCPs available, same pattern.
- **BookStack** — human self-hosted wiki; MCP plugin is an afterthought.
- **Wiki.js** — human wiki; community MCPs exist.
- **Obsidian** — human PKM; many MCPs (cyanheads/obsidian-mcp-server, MarkusPfundstein, etc.) but Obsidian itself is a desktop app for humans. (Note: Basic Memory uses Obsidian-compatible files, but the *primary* interface is MCP — that one passes.)
- **Logseq** — human PKM; same story as Obsidian.
- **Mintlify (as a docs product)** — human-authored docs site; the [llms.txt / agent-content-negotiation work](https://www.mintlify.com/blog/context-for-agents) is in scope as a standard, the product itself is not.
