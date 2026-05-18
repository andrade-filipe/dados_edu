---
description: Execute an approved plan task-by-task. Stability-first — pauses at formulas, theoretical concepts, and glossary definition edits. Conventional commits per task.
argument-hint: [caminho-do-plano]
---

Use the **implement** subagent to execute the plan.

**Plan:** $ARGUMENTS

If `$ARGUMENTS` is empty, the agent will use the most recent file in `docs/superpowers/plans/`.

The agent will:
1. Read the plan and the slices it references.
2. Execute each `- [ ]` step in order, marking `- [x]` as done.
3. Pause and ask for confirmation before touching: anything in `docs/dominio/formulas/`, `docs/dominio/conceitos/teoria-*.md`, `docs/dominio/conceitos/escala-saepe.md`, `docs/dominio/conceitos/descritores.md` (rule changes), and `docs/dominio/GLOSSARIO.md` (definition edits).
4. Commit per task using Conventional Commits. Never push.
5. At end, summarize commits + completed tasks + blockers and recommend `/verify-and-doc`.
