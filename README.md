# dados_edu

> Repositório dedicado ao projeto educacional em conjunto com **Alikaeli** (professor de Língua Portuguesa, 8º/9º anos) e **Caleb Carneiro**.

Buscamos transformar a análise pedagógica que hoje é feita manualmente em planilhas — descritores, agrupamento por nível, intervenções, relatórios — em um sistema que receba os dados das avaliações e gere automaticamente:

- análise de desempenho por descritor;
- percentual de acerto por questão;
- agrupamento de alunos por nível;
- identificação de descritores críticos;
- relatório pedagógico individual e por turma.

O contexto original do projeto está no arquivo [`post_alikaeli`](./post_alikaeli) (texto do professor explicando como o trabalho é feito hoje).

## Status

**Pré-implementação.** Esta fase é dedicada a **capturar e organizar o conhecimento de domínio** (pedagogia, avaliação, TCT, TRI simplificada, escala SAEPE) antes de qualquer decisão de arquitetura técnica. A primeira entrega é a fundação de documentação que você está lendo.

## Onde está cada coisa

Veja [`docs/INDEX.md`](./docs/INDEX.md) para o mapa completo. Atalho:

| Você é…                                    | Comece em                                                          |
|--------------------------------------------|--------------------------------------------------------------------|
| Professor contribuindo conteúdo            | [`docs/dominio/README.md`](./docs/dominio/README.md)               |
| Dev entrando no projeto                    | [`docs/dev/README.md`](./docs/dev/README.md)                       |
| Quer entender o workflow com IA            | [`docs/dev/AI-WORKFLOW.md`](./docs/dev/AI-WORKFLOW.md)             |
| Procurando vocabulário do projeto          | [`docs/dominio/GLOSSARIO.md`](./docs/dominio/GLOSSARIO.md)         |

## Workflow

Adotamos **Research → Plan → Implement → Verify** desde o início, via os slash commands `/research`, `/plan`, `/implement` e `/verify-and-doc` definidos em `.claude/`. Detalhes em [`docs/dev/AI-WORKFLOW.md`](./docs/dev/AI-WORKFLOW.md).

Em uma frase: antes de pedir qualquer mudança não-trivial ao agente, rode `/research <tópico>` — economiza rodadas.

## Áreas sensíveis

Algumas partes da documentação alteram análise pedagógica direta (fórmulas TRI/TCT, definições teóricas, escala SAEPE, descritores). Mexer nelas pede pausa para revisão. A lista exata vive em [`docs/dev/COMO-CONTRIBUIR.md`](./docs/dev/COMO-CONTRIBUIR.md).

## Convenções

- **Commits:** [Conventional Commits](https://www.conventionalcommits.org/).
- **Datas:** ISO `YYYY-MM-DD`.
- **Header de doc:** `> Última atualização: AAAA-MM-DD · Mantenedor: <quem>`.
- **PT-BR para domínio**, EN para conceitos técnicos quando houver código.
