---
description: Verify implementation against docs/llm/, update affected slices, create new slices for new areas, refresh last-verified dates. Edits only LLM docs and glossary additions.
---

Use the **verify-and-document** subagent to close the R-P-I-V loop.

The agent will:
1. Run `git diff main...HEAD --name-only` to find changed files.
2. Match each changed file against `owners-paths` declared in slice frontmatter (`docs/llm/slices/*.md`, `docs/llm/MAP.md`).
3. For each affected slice: confirm the slice still matches the source, update sections that drifted, bump `version` if behavior changed, refresh `last-verified` to today.
4. If the diff introduced an undocumented area, create a new slice following `docs/llm/conventions/writing-slices.md` and add a row to the table in `MAP.md`.
5. If new domain jargon appeared in the diff, add an entry to **both** `docs/dominio/GLOSSARIO.md` and `docs/llm/GLOSSARY.md`.
6. Run informational sanity checks (Conventional Commits format, "Última atualização" headers, stray `.md` files).
7. Output a structured report.

The agent **never** edits `docs/dominio/conceitos/`, `docs/dominio/processos/`, or `docs/dominio/formulas/` content beyond glossary additions — that's professor/team territory.
