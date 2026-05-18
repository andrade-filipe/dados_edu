---
description: Turn a research synthesis (or a clear feature request) into a step-by-step executable plan saved to docs/superpowers/plans/.
argument-hint: <feature ou tópico>
---

Use the **plan** subagent to design an implementation plan for:

**Feature:** $ARGUMENTS

The agent will:
1. Load `docs/llm/MAP.md`, `docs/llm/GLOSSARY.md`, and any slices the prior research recommended (or matching slices if no prior research).
2. Produce a bite-sized task list with exact file paths, content, and commit steps.
3. Mark stability-critical tasks (formulas, theoretical concepts, glossary definitions) with ⚠️.
4. Save the plan to `docs/superpowers/plans/YYYY-MM-DD-<feature-slug>.md`.

If the feature was not preceded by `/research` and is non-trivial, the agent will recommend running `/research` first.

The agent does not edit `docs/dominio/` content or future `src/`.
