# Índice da Documentação

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Este arquivo é o mapa-mor da documentação. Cada pasta abaixo tem uma responsabilidade clara — não duplique conteúdo entre elas.

## Estrutura

| Pasta                  | Para quem            | O que vive aqui                                                                              |
|------------------------|----------------------|----------------------------------------------------------------------------------------------|
| [`dominio/`](./dominio/)         | Professor + dev + LLM | **Fonte canônica** do conhecimento pedagógico: conceitos, processos, fórmulas, glossário.   |
| [`dev/`](./dev/)                 | Devs humanos          | Documentação de código (arquitetura, contribuição, workflow com IA). Cresce com o projeto.   |
| [`llm/`](./llm/)                 | Agente LLM            | Slices compactos sob demanda. Derivado de `dominio/` e (futuramente) do código.              |
| [`superpowers/specs/`](./superpowers/specs/)   | Time inteiro | Saídas de brainstorming — designs aprovados antes de virar plano executável.                |
| [`superpowers/plans/`](./superpowers/plans/)   | Time inteiro | Planos passo-a-passo gerados a partir de specs ou pedidos diretos. Consumidos por `/implement`. |

### Tooling auxiliar

Além de `docs/`, o projeto tem `.claude/` com agents, commands e skills que automatizam workflows recorrentes. Em particular:

- `.claude/agents/` — subagentes (research, plan, implement, verify-and-document).
- `.claude/commands/` — slash commands (`/research`, `/plan`, `/implement`, `/verify-and-doc`).
- `.claude/skills/` — skills project-level: `documentar-conceito`, `documentar-processo`, `atualizar-glossario`, `revisar-marcadores-todo`, `evoluir-tooling`.

Para entender quando cada um se aciona, ver [`dev/AI-WORKFLOW.md`](./dev/AI-WORKFLOW.md).

## Por onde começar

| Você é…                          | Comece em                                              |
|----------------------------------|--------------------------------------------------------|
| Novo dev no projeto              | [`dev/README.md`](./dev/README.md) e depois [`dev/AI-WORKFLOW.md`](./dev/AI-WORKFLOW.md) |
| Professor contribuindo conteúdo  | [`dominio/README.md`](./dominio/README.md)             |
| Agente LLM em /research          | [`llm/MAP.md`](./llm/MAP.md) (já carregado pelo agente) |
| Procurando vocabulário PT-BR     | [`dominio/GLOSSARIO.md`](./dominio/GLOSSARIO.md)       |

## Princípios

1. **Domínio é canônico.** Toda afirmação pedagógica/matemática vive em `dominio/`. Outras pastas referenciam, não duplicam.
2. **Slices LLM são derivados.** `llm/slices/*` é compacto e cita `dominio/` como fonte. `last-verified` no frontmatter sinaliza staleness.
3. **`dev/` cresce com o código.** Não existem placeholders vazios; quando definirmos arquitetura, ela ganha seu doc.
4. **Templates substituem markdown.** Em `dominio/_templates/` há esqueletos para o professor preencher sem precisar dominar a sintaxe.

## Convenções

- Datas em ISO `YYYY-MM-DD`.
- Header `> Última atualização: AAAA-MM-DD · Mantenedor: <pessoa/time>` no topo de todo doc de primeiro nível.
- Commits seguem [Conventional Commits](https://www.conventionalcommits.org/).
- Mudanças em fórmulas, definições teóricas ou descritores pedem revisão do professor antes do commit.

## Workflow Research → Plan → Implement → Verify

Veja [`dev/AI-WORKFLOW.md`](./dev/AI-WORKFLOW.md) para o tutorial completo. Em resumo: `/research` antes de mexer em área desconhecida; `/plan` antes de implementar não-trivial; `/implement` executa; `/verify-and-doc` sincroniza docs com a realidade.
