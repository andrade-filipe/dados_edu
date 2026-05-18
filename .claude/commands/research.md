---
description: Investigate a topic in this repository. Loads MAP.md + GLOSSARY.md + matching slices, traces docs/dominio/ paths, returns mental model. Read-only.
argument-hint: <tópico>
---

Use the **research** subagent to investigate the following topic in this repository:

**Topic:** $ARGUMENTS

The agent will:
1. Load `docs/llm/MAP.md` and `docs/llm/GLOSSARY.md`.
2. Match the topic against slice triggers and load 0–3 relevant slices.
3. Read the referenced files in `docs/dominio/` (or `src/` once it exists).
4. Return a structured synthesis: files of interest, mental model, open questions, and slices to load if implementing.

Do not write any code or docs. This is research only.
