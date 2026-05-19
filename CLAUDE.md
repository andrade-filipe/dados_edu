# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

**Pre-implementation, docs-only.** No `src/` exists yet. The project is capturing pedagogical/psychometric domain knowledge (TCT, simplified TRI, SAEPE scale, BNCC, SAEB descriptors) *before* any architecture decision. The first deliverable is the documentation foundation in `docs/`.

Collaborators include a non-dev domain expert (prof. Alikaeli). Most contributions to `docs/dominio/` come from him via guided skills, not direct markdown edits.

## Default workflow: R-P-I-V

Use the four slash commands instead of ad-hoc edits for anything non-trivial. Each maps to a subagent in `.claude/agents/`:

- `/research <tópico>` — read-only investigation. Loads `docs/llm/MAP.md` + `GLOSSARY.md` + relevant slices, traces `docs/dominio/`, returns a synthesis. **Run this before any non-trivial change.**
- `/plan <feature>` — turns research (or a direct request) into a step-by-step plan saved to `docs/superpowers/plans/YYYY-MM-DD-<topic>.md`.
- `/implement` — executes the plan task-by-task, Conventional Commits per task, **pauses for human confirmation on stability-critical files** (see below).
- `/verify-and-doc` — diffs the work against `docs/llm/`, updates affected slices via `owners-paths`, refreshes `last-verified`. Edits only LLM docs + glossary additions; never rewrites domain content unilaterally.

Skip R-P-I-V only for typos, renames, single-term glossary additions, and trivially-verified factual fixes. See `docs/dev/AI-WORKFLOW.md` for the full tutorial.

## Stability-critical areas (pause for confirmation)

`/implement` and any direct edit must pause before touching:

- `docs/dominio/formulas/**` — math (TRI simplified, TCT, etc.).
- `docs/dominio/conceitos/teoria-*.md` — theoretical foundations.
- `docs/dominio/GLOSSARIO.md` — when *modifying* a definition (adding a new term is fine).
- `.claude/agents/**`, `.claude/commands/**`, `.claude/skills/**` — tooling errors propagate to all future work.

Definitive list in `docs/dev/COMO-CONTRIBUIR.md`. When code is added later, more paths will join this list.

## Documentation architecture (non-obvious)

Two parallel views of the same knowledge — **don't duplicate manually**:

- `docs/dominio/` — **canonical source**, PT-BR, narrative for humans. Every pedagogical/mathematical claim lives here.
- `docs/llm/slices/*.md` — compact, telegraphic, agent-facing slices derived from `dominio/`. Each has frontmatter (`when-to-load`, `owners-paths`, `last-verified`); ≤200 lines. Cite `dominio/` as source.

`docs/llm/MAP.md` is always loaded by `/research` and `/plan` — it lists slice triggers ("if the topic contains X, load slice Y"). When you add a new concept to `dominio/`, run `/verify-and-doc` so the slice mirror and MAP triggers stay in sync.

Two glossaries: `docs/dominio/GLOSSARIO.md` (complete, human) and `docs/llm/GLOSSARY.md` (compact, agent). Domain version wins; use the `atualizar-glossario` skill to update both atomically.

**"TRI" in this project means the simplified TRI we use, not the official ENEM/SAEB three-parameter TRI.** When in doubt, say which.

## Project skills (`.claude/skills/`)

Project-level skills that the agent invokes when context matches (not user-triggered):

- `documentar-conceito`, `documentar-processo` — guided PT-BR Q&A to help the professor fill a `_templates/` skeleton.
- `atualizar-glossario` — sync a new term across both glossaries atomically.
- `revisar-marcadores-todo` — walk and resolve HTML review markers (see below).
- `evoluir-tooling` — detect drift between `.claude/` and the repo's reality. Stability-critical; one diff at a time, confirm each.

Don't invent new skills ad-hoc — brainstorm + `/plan` first.

## Review markers

Two HTML markers signal content needing attention. The distinction matters because the effort differs:

| Marker | Meaning | Professor's job |
|---|---|---|
| `<!-- precisa de revisão do professor: <pergunta> -->` | Content is missing — a dev/IA left a hole only the professor can fill | **Create** content from scratch |
| `<!-- EXTRAÍDO de fonte oficial em AAAA-MM-DD · Fonte: <URL> · Status: aguarda validação pedagógica. -->` | Content present, transcribed from an official source | **Validate** transcription, adjust nuance, remove marker |

## Conventions

- **Commits:** [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `refactor:`, …). One coherent set of tasks per PR.
- **Dates:** ISO `YYYY-MM-DD`.
- **Top-level doc header:** `> Última atualização: AAAA-MM-DD · Mantenedor: <quem>`.
- **Language:** PT-BR for domain content; EN for technical/code concepts when code exists.
- **File paths in docs:** always exact (`docs/dominio/conceitos/foo.md`, `src/foo.js:123`).
- **No empty placeholders.** A doc is created only when it has real content.

## External sources

Only official sources or peer-reviewed academic publications belong in `docs/dominio/REFERENCIAS.md` (SAEPE, INEP, BNCC, nota técnica TRI, etc.). When transcribing from one, apply the "EXTRAÍDO de fonte oficial" marker above so the professor knows to validate.
