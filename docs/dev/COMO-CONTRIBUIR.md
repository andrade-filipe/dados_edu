# Como contribuir

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Guia rápido para devs e não-devs.

## Para o professor (não-dev)

Você é a fonte da verdade do conteúdo pedagógico. Veja o guia detalhado em [`../dominio/README.md`](../dominio/README.md).

Em resumo:
1. Use os templates em `docs/dominio/_templates/`.
2. Se não souber editar markdown, peça ao Claude para preencher o template a partir da sua explicação.
3. Mudanças em fórmulas pedem revisão antes de virar permanente.

## Para devs

### Antes de mexer

1. Leia [`AI-WORKFLOW.md`](./AI-WORKFLOW.md).
2. Para qualquer mudança não-trivial, comece com `/research <tópico>`.
3. Para feature/refactor: `/plan` antes de `/implement`.

### Convenções

- **Commits:** [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `refactor:`, …).
- **Headers de doc:** todo arquivo top-level tem `> Última atualização: AAAA-MM-DD · Mantenedor: <quem>`.
- **PT-BR para domínio.** EN para conceitos técnicos quando houver código.
- **Datas em ISO.**

### Áreas sensíveis (pausa obrigatória)

`/implement` pausa antes de tocar em:

- `docs/dominio/formulas/**` — matemática do projeto.
- `docs/dominio/conceitos/teoria-*.md` — fundamentos teóricos.
- `docs/dominio/GLOSSARIO.md` — quando edição altera definição (não adição de termo novo).

Quando começar implementação real (código), a lista vai crescer.

### Pull Requests

- Branch a partir de `main`.
- Um conjunto coerente de tasks por PR.
- Descrição cita os specs/plans relacionados.
- Rode `/verify-and-doc` antes de abrir o PR para garantir slices atualizadas.
