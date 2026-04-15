---
description: Bootstrap or check the patent wiki vault. Reads the patent-wiki skill and runs setup or routing.
---

Read the `patent-wiki` skill. Then:

1. Check if this directory has a wiki/ folder with content. If yes, report current vault state (patents ingested, page count, last operation).
2. If the wiki is not scaffolded, run the SCAFFOLD operation.
3. If the wiki is ready, ask: "What would you like to do? Ingest a patent, query the wiki, or run a health check?"

Examples of what the user might say:
- "Ingest US20260072718A1"
- "What claims does the code interpreter patent have?"
- "Compare the three OpenAI patents"
- "Run a health check"
- "What are the risks for Claim 21?"

Route to the appropriate sub-skill based on the user's intent.
