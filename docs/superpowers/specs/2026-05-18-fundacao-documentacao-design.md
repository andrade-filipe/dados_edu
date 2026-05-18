# Fundação de Documentação — Design

> Spec gerado por brainstorming · 2026-05-18 · Autor: Filipe Andrade (com Claude)

## Contexto

Repositório `dados_edu` é um projeto educacional em fase pré-implementação, em colaboração com o professor Alikaeli (autor do post em `post_alikaeli` na raiz) e Caleb Carneiro. O objetivo final é construir um sistema/API que automatize análise pedagógica que hoje é feita manualmente em planilhas — análise de descritores, agrupamento de alunos por nível, identificação de descritores críticos, geração de relatórios.

O projeto começa **sem código**. A primeira fase é capturar e organizar conhecimento de domínio (pedagogia, avaliação, TCT, TRI simplificado, escala SAEPE) de forma que tanto humanos quanto LLMs consigam navegar com baixo atrito.

A referência usada como ponto de partida (`2026-05-08-dev-docs-and-readme.md`) foi um plano de documentação de **código existente** (PWA Zoetis). A natureza greenfield deste projeto inverte prioridades: o domínio é a fundação, não a arquitetura técnica.

## Goal

Estabelecer estrutura de documentação que:
- Sirva como **fonte canônica de domínio** para o time inteiro (incluindo o professor, que não é dev).
- Permita workflow **Research → Plan → Implement → Verify** desde o dia zero, com slices LLM-first carregados sob demanda.
- Cresça organicamente: o lado `dev/` é leve hoje e expande quando começarmos a implementar.
- Permita o professor contribuir sem dominar Markdown técnico — via templates e estrutura clara.

## Non-goals

- Não definir arquitetura técnica ainda (será resultado de Research/Plan posterior).
- Não escrever código aplicacional nesta fase.
- Não traduzir vocabulário pedagógico para inglês — domínio é PT-BR.
- Não criar geradores estáticos de site, índices automáticos ou pipelines de doc — markdown puro.

## Estrutura proposta

```
docs/
├── INDEX.md                  # mapa-mor: o que cada pasta serve
├── dominio/                  # fonte canônica de conhecimento pedagógico
│   ├── README.md             # guia de navegação + onboarding do professor
│   ├── GLOSSARIO.md          # termos PT-BR com definições e exemplos
│   ├── conceitos/            # teoria — um conceito por arquivo
│   ├── processos/            # como o professor trabalha hoje
│   ├── formulas/             # matemática real (TRI/TCT/SAEPE)
│   └── _templates/           # esqueletos para o professor preencher
├── dev/                      # documentação dev — magra até existir código
│   ├── README.md
│   ├── AI-WORKFLOW.md        # tutorial R-P-I-V adaptado
│   └── COMO-CONTRIBUIR.md    # inclui guia para não-devs
├── llm/                      # LLM-first compacto, sob demanda
│   ├── MAP.md                # sempre carregado: índice + triggers
│   ├── GLOSSARY.md           # versão compacta do GLOSSARIO de domínio
│   ├── slices/               # ≤200 linhas, frontmatter when-to-load
│   └── conventions/          # writing-slices, context-engineering
└── superpowers/
    ├── specs/                # saída de brainstorming
    └── plans/                # saída de /plan
```

## Decisões de design

### D1 — Dominio como fonte canônica única

Toda afirmação pedagógica/matemática vive em `docs/dominio/`. `docs/llm/` é **derivado** (compacto, otimizado para carga de contexto). `dev/` cita o domínio quando precisa.

**Por quê:** Evita drift entre múltiplas cópias. Quando o professor corrigir um conceito, ele edita em um único lugar; o slice LLM é atualizado por `/verify-and-doc`.

**Trade-off:** Slices LLM podem ficar desatualizados em relação ao domínio. Mitigação: campo `last-verified` no frontmatter dos slices, `/verify-and-doc` valida.

### D2 — Dois glossários (não é duplicação)

- `dominio/GLOSSARIO.md` — completo, com exemplos, frases de uso, referências de origem. Otimizado para leitura humana.
- `llm/GLOSSARY.md` — compacto, uma linha por termo. Otimizado para sempre carregar em contexto LLM.

**Por quê:** A trade-off entre legibilidade humana e densidade para LLM são opostas. Tentar servir os dois com um único arquivo gera ou um doc verboso demais para carregar sempre, ou seco demais para o professor.

### D3 — `dev/` sem placeholders vazios

Não criar `ARQUITETURA.md` agora. Só existirá quando houver arquitetura.

**Por quê:** Placeholders ficam stale e enganam o leitor. "Documento existe" ≠ "documento útil".

### D4 — Templates no domínio para o professor

`docs/dominio/_templates/novo-conceito.md` e `novo-processo.md` dão estrutura preenchível. Reduzem barreira de entrada.

**Por quê:** O professor é a fonte da verdade do domínio, mas não é dev. Templates substituem "preciso aprender markdown" por "preciso preencher esses 5 campos".

### D5 — Slices iniciais espelham os conceitos do domínio

Cada conceito grande em `dominio/conceitos/` ou `dominio/processos/` recebe um slice equivalente compacto em `llm/slices/`. Slice cita arquivo de domínio com link.

**Por quê:** R-P-I-V eficiente exige slices carregados sob demanda. Sem slices iniciais, o `/research` cai em `MAP.md` + `GLOSSARY.md` apenas — perde precisão.

### D6 — Pontos sensíveis ("stability-critical" adaptado)

No Zoetis, eram sync worker, schema, migrations. Aqui são:

- **Fórmulas matemáticas** (TRI simplificado, TCT, ajuste de chute, conversão SAEPE) — erro = análise errada do desempenho.
- **Mapeamento de descritores** — referencial curricular oficial.
- **Definições de níveis** (fácil/médio/difícil, leitor proficiente vs. iniciante).

`/implement` adaptado deve pausar e pedir confirmação humana (idealmente do professor) antes de tocar nesses.

### D7 — `.claude/` reescrito de cabo a rabo

Os 8 arquivos atuais (`agents/*` e `commands/*`) ainda têm referências a IATF, sync worker, WatermelonDB, schema. Não dá para fazer Find/Replace — preciso reescrever cada um para o contexto educacional.

**Mantém:** estrutura R-P-I-V, frontmatter, formato de output, hard rules.
**Troca:** referências técnicas, lista de pontos sensíveis, glossário esperado, paths.

### D8 — README como vitrine, não tutorial

README explica em 30 segundos: o que é, qual o contexto humano (post Alikaeli), onde está cada coisa. Aponta para `docs/INDEX.md` para a estrutura completa.

**Por quê:** Quem chega no repo (GitHub, recomendação de amigo) precisa de uma vitrine. Tutorial vai em `dev/`.

## Conteúdo inicial extraído do post

Material concreto disponível em `post_alikaeli` para semear o domínio:

**Conceitos:**
- Avaliação diagnóstica / simulado
- Descritor (habilidade curricular)
- Teoria Clássica dos Testes (TCT) — classificação fácil/médio/difícil por % de erro
- Teoria de Resposta ao Item (TRI) simplificada — penalização de chute
- Escala SAEPE (0 a 500)
- Sequência didática de intervenção
- Curadoria de gêneros textuais
- Agrupamento por perfil/nível
- Pensamento autônomo e reflexivo

**Processos:**
- Aplicação de avaliações em 8º/9º anos
- Coleta de dados (planilhas com colunas AN-AZ mapeadas)
- Análise por descritor e por questão
- Identificação de descritores críticos
- Geração de relatórios pedagógicos (por aluno e por turma)

**Fórmulas explícitas no post:**
```
Penalização do acerto ao acaso:
  =($AN$1-AN3)-(AN3/3)        # fáceis
  =($AP$1-AP3)-(AP3/3)        # médias
  =($AR$1-AR3)-(AR3/3)        # difíceis

Ajuste para questões fáceis:
  =SE((13-AN3)/13 < 0,5; MÍNIMO(AV3;0) + MÁXIMO(AV3;0)*0,5; AV3)

Nota real ponderada:
  =MÁXIMO(AT3;0)*1 + MÁXIMO(AU3;0)*1,5 + AW3*2

Classificação TCT:
  erro > 50%       → difícil
  50% < acerto ≤ 70% → médio
  acerto > 70%     → fácil
```

**Mapeamento de colunas (planilha original):**
```
AN/AO  erros fáceis / %
AP/AQ  erros médias / %
AR/AS  erros difíceis / %
AT/AU/AV  pontos corrigidos F/M/D
AW     ajuste de pontos das difíceis
AX     nota real ponderada
AY     nota em %
AZ     nota convertida em SAEPE (0-500)
```

Esse material vai diretamente para `dominio/formulas/tri-simplificado.md` (com explicação por trás) e `dominio/conceitos/teoria-classica-testes-tct.md` / `teoria-resposta-item-tri.md`.

## Workflow R-P-I-V adaptado

| Etapa | Zoetis (referência) | dados_edu (nosso) |
|-------|---------------------|---------------------|
| Research | Investiga código existente | Investiga **domínio** (e código depois que houver) |
| Plan | Gera plano com paths de `src/` | Gera plano com paths de `docs/` ou `src/` quando aplicável |
| Implement | Edita código com pausas em sync/schema | Edita docs/código com pausas em **fórmulas/descritores/níveis** |
| Verify | Sincroniza `docs/llm/` com `src/` | Sincroniza `docs/llm/` com `docs/dominio/` (e depois `src/`) |

## Critérios de sucesso

- [ ] `docs/INDEX.md` permite ao novo dev entender o repo em <2 minutos.
- [ ] Professor consegue contribuir um conceito novo usando `_templates/` sem ajuda de dev.
- [ ] `/research <termo do domínio>` retorna mental model útil carregando ≤3 slices.
- [ ] Nenhum link cruzado quebrado entre `README.md` ↔ `docs/INDEX.md` ↔ `docs/{dominio,dev,llm}/`.
- [ ] `.claude/agents/*` e `.claude/commands/*` não contêm mais referências a Zoetis/IATF/sync worker.
- [ ] O conteúdo do `post_alikaeli` está integralmente refletido em `docs/dominio/` (nenhum conceito ou fórmula perdido).
- [ ] Cabeçalho "Última atualização" presente em todo doc top-level.

## Riscos e mitigações

| Risco | Mitigação |
|-------|-----------|
| Slices LLM ficam desatualizados em relação ao domínio | Campo `last-verified` + execução periódica de `/verify-and-doc` |
| Professor cria conteúdo que não casa com template | Templates incluem comentários explicativos por seção |
| `dev/` cresce sem disciplina quando começar implementação | Convenção: cada doc novo em `dev/` precisa de header com data e mantenedor |
| R-P-I-V em greenfield vira excesso de processo para tarefa trivial | `AI-WORKFLOW.md` explicita "quando NÃO usar R-P-I-V" |

## Plano de implementação

Detalhado em [`docs/superpowers/plans/2026-05-18-fundacao-documentacao.md`](../plans/2026-05-18-fundacao-documentacao.md).

Ordem de alto-nível:
1. Esqueleto de pastas + `docs/INDEX.md`
2. `docs/dominio/` populado a partir do `post_alikaeli`
3. `docs/llm/` (MAP + GLOSSARY + convenções + slices)
4. `docs/dev/` (3 arquivos)
5. Adaptar `.claude/agents/*` e `.claude/commands/*`
6. Reescrever `README.md`
7. Verificação cruzada final
