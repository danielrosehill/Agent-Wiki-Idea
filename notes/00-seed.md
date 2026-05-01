# Seed thought

The wiki of tomorrow might serve as an information repository where it's assumed that the main users will be agents.

For that reason, it might be **fully vectorised**: skills are provided that upsert new data and search against it semantically. There is no "edit this page" button — there are tool calls.

Things to flesh out next:
- Minimal viable skill surface (`upsert_chunk`, `search`, `deprecate`, `link`).
- Backing store options: Pinecone, pgvector, Qdrant, Chroma, Weaviate.
- What does the MCP server for this look like?
- Compare to: llms.txt, agents.md, RAG-as-a-service products.
