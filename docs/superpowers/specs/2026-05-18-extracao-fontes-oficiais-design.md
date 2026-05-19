# Extração de Fontes Oficiais — Design

> Spec gerado por brainstorming · 2026-05-18 · Autor: Filipe Andrade (com Claude)

## Contexto

`docs/dominio/` foi semeado a partir do post informal do prof. Alikaeli (já removido) e tem marcadores `<!-- precisa de revisão do professor -->` em pontos onde o post não detalhava. Esses marcadores assumem que o professor vai preencher do zero.

Mas há um trabalho que o **dev pode fazer sozinho**, aproveitando as fontes oficiais já catalogadas em [`docs/dominio/REFERENCIAS.md`](../../dominio/REFERENCIAS.md): **transcrever conteúdo autoritativo** (lista de descritores da Matriz SAEB, estrutura dos níveis SAEPE, modelo 3-parâmetros da TRI, codificação BNCC) e estruturar nos arquivos de domínio. Isso transforma a tarefa do professor de "criar do zero" em "validar pedagogicamente uma transcrição já feita".

Princípio: **dev transcreve com fidelidade absoluta + cita fonte; professor valida pedagogia e adiciona contexto** (exemplos de turma, nuances regionais, prioridades curriculares).

## Goal

Pré-popular três conceitos existentes e criar um novo, usando material de fontes oficiais, de forma que:
1. O conteúdo extraído seja claramente marcado como aguardando validação pedagógica (não confundido com claim do professor).
2. Cada bloco transcrito tenha cita URL + data de extração para rastreabilidade.
3. As slices LLM correspondentes sejam atualizadas para refletir a nova densidade do domínio.

## Non-goals

- Não interpretar conteúdo pedagógico. Dev transcreve fielmente; análise/exemplificação é tarefa do professor.
- Não criar tudo que está nos PDFs oficiais — apenas o que diretamente endereça lacunas atuais no domínio.
- Não criar `conceitos/matriz-saeb.md` separado do `descritores.md` (hoje é mesmo conceito).
- Não substituir os marcadores `<!-- precisa de revisão do professor -->` existentes nos conceitos — eles continuam válidos para os pontos que dependem da voz do professor.

## Nova convenção de marcação

Adicionar um segundo marcador distinto:

```markdown
<!-- EXTRAÍDO de fonte oficial em AAAA-MM-DD · Fonte: <URL>
     Status: aguarda validação pedagógica do professor. -->
```

Diferença semântica em relação ao marcador existente:

| Marcador | Significado | Ação do professor |
|---|---|---|
| `<!-- precisa de revisão do professor -->` | Conteúdo ausente, professor cria do zero | Preenche |
| `<!-- EXTRAÍDO de fonte oficial... -->` | Conteúdo presente (transcrito), aguarda olhar pedagógico | Confirma, ajusta nuance, remove marcador |

Convenção documentada em [`dominio/README.md`](../../dominio/README.md) (atualizar nesta entrega).

## Escopo de extração

### 1. `conceitos/descritores.md` — tabela completa D1-D21

Material disponível: lista oficial da Matriz de Referência SAEB de Língua Portuguesa para 9º ano EF, organizada em 6 tópicos.

Fonte: [INEP gov.br](https://www.gov.br/inep/pt-br/centrais-de-conteudo/acervo-linha-editorial/publicacoes-institucionais/avaliacoes-e-exames-da-educacao-basica/matriz-de-referencia-de-lingua-portuguesa-saeb), via curadoria QEdu.

Estrutura a inserir (substitui ou complementa a seção atual "Como funciona"):

| Tópico | Descritores |
|---|---|
| I. Procedimentos de Leitura | D1, D3, D4, D6, D14 |
| II. Implicações do Suporte, Gênero, Enunciador | D5, D12 |
| III. Relação entre Textos | D20, D21 |
| IV. Coerência e Coesão | D2, D7, D8, D9, D10, D11, D15 |
| V. Recursos Expressivos e Efeitos de Sentido | D16, D17, D18, D19 |
| VI. Variação Linguística | D13 |

Total: 21 descritores. Cada um com nome oficial completo. Tabela cita fonte INEP + data.

### 2. `conceitos/escala-saepe.md` — estrutura dos 4 níveis

Material disponível: SAEPE adota 4 padrões de desempenho — **Elementar I, Elementar II, Básico, Desejável** (estrutura confirmada via múltiplas fontes CAEd).

Pontos de corte exatos para LP 9º ano: dependem do relatório oficial SAEPE 2021 ([PE2021RPLP.pdf](https://avaliacaoemonitoramentopernambuco.caeddigital.net/resources/arquivos/colecoes/SAEPE_2021/PE2021RPLP.pdf), 6.1MB) — não consegui parsear via WebFetch. Esse passo fica como TODO específico citando o PDF e a página a consultar.

Estrutura a inserir:

- Definição dos 4 níveis (nome + descrição pedagógica genérica do que cada um significa).
- Tabela com colunas "Nível | Pontuação (LP 9º ano)" — pontuação marcada como TODO de consulta ao PDF.
- Nota: para Matemática 9º ano, os pontos de corte encontrados (Elementar I: ≤225, Elementar II: 225-245, Básico: 245-280, Desejável: >280) podem servir de placeholder de estrutura, mas explicitamente sem afirmar que são para LP.

### 3. `conceitos/teoria-resposta-item-tri.md` — modelo 3-parâmetros

Material disponível: modelo logístico de 3 parâmetros do INEP/ENEM:

- **a** (discriminação): mede quanto a questão distingue alunos com habilidades diferentes.
- **b** (dificuldade): ponto na escala onde a probabilidade de acerto é metade do valor entre o acerto ao acaso e 1.
- **c** (acerto ao acaso): probabilidade de acerto por chute (típico 0.20-0.25 em prova de 5 alternativas).

Fonte: [Nota Técnica TRI ENEM (INEP, 2011)](https://download.inep.gov.br/educacao_basica/enem/nota_tecnica/2011/nota_tecnica_tri.pdf).

Inserção: nova seção "Modelo oficial vs nosso modelo simplificado" em `teoria-resposta-item-tri.md`, com tabela comparativa:

| Parâmetro | TRI oficial (INEP/ENEM) | TRI simplificada (este projeto) |
|---|---|---|
| Discriminação (a) | Estimada por calibração estatística | Não estimada (assume itens equivalentes na mesma faixa) |
| Dificuldade (b) | Estimada por calibração | Aproximada via TCT (% de erro da turma) |
| Acerto ao acaso (c) | Estimado por calibração | Heurística fixa (1/3 dos erros) |

### 4. `conceitos/habilidades-bncc.md` — novo conceito

Material disponível: codificação BNCC `EFXXLPNN` (Ensino Fundamental, ano, Língua Portuguesa, número) + exemplos para 9º ano.

Fonte: [BNCC PDF oficial](https://basenacionalcomum.mec.gov.br/images/BNCC_20dez_site.pdf).

Conteúdo a criar:

- O que é a BNCC.
- Codificação das habilidades.
- Exemplos de habilidades EF09LP (3-5 amostras transcritas literalmente, com citação).
- Como BNCC dialoga com a Matriz SAEB (não substitui — coexistem).

### 5. Sincronia LLM

Slices a atualizar para refletir a nova densidade:

- `docs/llm/slices/dominio-descritores.md` — incluir contagem (21 descritores) e os 6 tópicos.
- `docs/llm/slices/dominio-tri-simplificado.md` — incluir referência ao modelo 3-parâmetros oficial.

Adicionar slice nova:

- `docs/llm/slices/dominio-habilidades-bncc.md` — espelho do novo conceito.

Atualizar `docs/llm/MAP.md` para incluir o trigger do novo slice.

## Critérios de sucesso

- [ ] `conceitos/descritores.md` lista os 21 descritores nominalmente, agrupados nos 6 tópicos.
- [ ] `conceitos/escala-saepe.md` tem os 4 níveis nomeados + faixa de pontuação marcada como TODO específico.
- [ ] `conceitos/teoria-resposta-item-tri.md` tem tabela comparativa TRI oficial vs simplificada.
- [ ] `conceitos/habilidades-bncc.md` existe e está linkado pelo `descritores.md` como "conceito relacionado".
- [ ] Toda transcrição tem o marcador `<!-- EXTRAÍDO ... -->` com URL e data.
- [ ] Slices LLM atualizadas com `last-verified: 2026-05-18` e version bumped.
- [ ] MAP.md tem trigger para o slice nova.
- [ ] `dominio/README.md` documenta o novo marcador (princípio de extração).

## Riscos e mitigações

| Risco | Mitigação |
|---|---|
| Dev transcrever errado e marcar como "fonte oficial" | Cada bloco cita URL exata + data → o professor consulta a fonte e valida em segundos |
| Marcador novo se confundir com o existente | Texto diferente, semântica documentada em `dominio/README.md` |
| Pontos de corte SAEPE LP virem errados quando preenchidos | TODO específico cita o PDF e a página; quem preencher consulta a fonte primária |
| BNCC e Matriz SAEB se misturarem | Conceitos separados (`descritores.md` e `habilidades-bncc.md`) com seção explícita de relação |
| Tabela de 21 descritores explode o tamanho do conceito | Aceitável — se passar de 200 linhas, dividir depois |

## Plano de implementação

Detalhado em [`docs/superpowers/plans/2026-05-18-extracao-fontes-oficiais.md`](../plans/2026-05-18-extracao-fontes-oficiais.md).

Ordem: `dominio/README.md` (documentar marcador) → 4 conceitos (3 atualizações + 1 criação) → slices LLM → MAP → verificação.
