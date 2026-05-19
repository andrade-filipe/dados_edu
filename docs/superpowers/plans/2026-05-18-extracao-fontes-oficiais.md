# Extração de Fontes Oficiais Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Pré-popular `docs/dominio/` com material transcrito de fontes oficiais (Matriz SAEB, SAEPE, Nota Técnica TRI, BNCC), marcado distintamente como "aguarda validação pedagógica", reduzindo a carga inicial do professor de "criar do zero" para "validar transcrição já estruturada".

**Architecture:** Dev faz transcrição autoritativa com cita de URL + data; professor depois faz validação pedagógica e remove marcador. Novo marcador `<!-- EXTRAÍDO ... -->` documentado em `dominio/README.md`.

**Tech Stack:** Markdown puro. Sem dependências.

**Spec:** [`docs/superpowers/specs/2026-05-18-extracao-fontes-oficiais-design.md`](../specs/2026-05-18-extracao-fontes-oficiais-design.md)

**Ordem de execução:**
1. Documentar nova convenção em `dominio/README.md`.
2. Atualizar 3 conceitos existentes + criar 1 novo (4 tasks).
3. Atualizar/criar slices LLM (2 tasks).
4. Atualizar MAP.md (1 task).
5. Verificação cruzada (1 task).

**Stability-critical:** Tasks 2-5 (mudanças em `conceitos/teoria-*.md` e `conceitos/descritores.md` e `formulas/` referenciadas indiretamente). Todas as edições têm conteúdo verbatim no plano — o stability-critical pede pausa só para evitar surpresa, não para reinterpretar.

---

## Task 1: Documentar nova convenção de marcador em `dominio/README.md`

**Files:**
- Modify: `docs/dominio/README.md`

- [x] **Step 1: Adicionar seção "Marcadores de revisão" antes de "Para o dev: como consumir"**

Ler `docs/dominio/README.md`. Localizar a seção `## Para o dev: como consumir`.

Inserir antes dela esta nova seção:

```markdown
## Marcadores de revisão

Dois marcadores HTML são usados para sinalizar conteúdo que precisa de atenção:

| Marcador | Quando aparece | O que o professor faz |
|---|---|---|
| `<!-- precisa de revisão do professor: <pergunta opcional> -->` | Conteúdo ausente — alguém (dev ou IA) deixou um buraco que só o professor pode preencher (exemplo de turma, faixa específica, etc.). | Cria o conteúdo do zero. |
| `<!-- EXTRAÍDO de fonte oficial em AAAA-MM-DD · Fonte: <URL> · Status: aguarda validação pedagógica. -->` | Conteúdo presente, transcrito por dev a partir de uma fonte oficial. | Confirma transcrição contra a fonte, ajusta nuance pedagógica, remove o marcador quando ok. |

A distinção importa: o primeiro requer **criação**; o segundo requer **validação**. Tempo e energia diferentes.

```

```

- [x] **Step 2: Commit**

```bash
git add docs/dominio/README.md
git commit -m "docs(dominio): documentar marcador EXTRAIDO para conteudo de fonte oficial"
```

---

## Task 2: Expandir `conceitos/descritores.md` com tabela completa D1-D21

⚠️ **STABILITY-CRITICAL** — conceito teórico. Conteúdo extraído verbatim de fonte oficial; pausar antes de aplicar para que o usuário confira o diff.

**Files:**
- Modify: `docs/dominio/conceitos/descritores.md`

- [x] **Step 1: Ler o arquivo atual**

Ler `docs/dominio/conceitos/descritores.md` para entender estrutura atual.

- [x] **Step 2: Substituir a seção "Como funciona" pela versão expandida**

Localizar a seção atual `## Como funciona` (entre `## Por que importa para o nosso projeto` e `## Exemplo concreto`). Substituir por:

```markdown
## Como funciona

1. Cada questão da avaliação é vinculada a um descritor (uma questão pode tocar mais de um, mas idealmente foca em um principal).
2. Após coleta, calcula-se o % de acerto **por descritor** (e não só por questão).
3. Descritores com desempenho baixo são marcados como "críticos" e priorizados na intervenção.

## Descritores oficiais — Matriz SAEB de Língua Portuguesa, 9º ano EF

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: https://www.gov.br/inep/pt-br/centrais-de-conteudo/acervo-linha-editorial/publicacoes-institucionais/avaliacoes-e-exames-da-educacao-basica/matriz-de-referencia-de-lingua-portuguesa-saeb · Status: aguarda validação pedagógica. -->

A Matriz de Referência do SAEB para Língua Portuguesa do 9º ano organiza 21 descritores em 6 tópicos. Cada descritor é uma habilidade leitora específica.

### I. Procedimentos de Leitura

| Descritor | Habilidade |
|---|---|
| D1 | Localizar informações explícitas em um texto |
| D3 | Inferir o sentido de uma palavra ou expressão |
| D4 | Inferir uma informação implícita em um texto |
| D6 | Identificar o tema de um texto |
| D14 | Distinguir um fato da opinião relativa a esse fato |

### II. Implicações do Suporte, do Gênero e/ou do Enunciador

| Descritor | Habilidade |
|---|---|
| D5 | Interpretar texto com auxílio de material gráfico diverso (propagandas, quadrinhos, foto, etc.) |
| D12 | Identificar a finalidade de textos de diferentes gêneros |

### III. Relação entre Textos

| Descritor | Habilidade |
|---|---|
| D20 | Reconhecer diferentes formas de tratar uma informação na comparação de textos que tratam do mesmo tema, em função das condições em que ele foi produzido e daquelas em que será recebido |
| D21 | Reconhecer posições distintas entre duas ou mais opiniões relativas ao mesmo fato ou ao mesmo tema |

### IV. Coerência e Coesão no Processamento do Texto

| Descritor | Habilidade |
|---|---|
| D2 | Estabelecer relações entre partes de um texto, identificando repetições ou substituições que contribuem para a continuidade de um texto |
| D7 | Identificar a tese de um texto |
| D8 | Estabelecer relação entre a tese e os argumentos oferecidos para sustentá-la |
| D9 | Diferenciar as partes principais das secundárias em um texto |
| D10 | Identificar o conflito gerador do enredo e os elementos que constroem a narrativa |
| D11 | Estabelecer relação de causa/consequência entre partes e elementos do texto |
| D15 | Estabelecer relações lógico-discursivas presentes no texto, marcadas por conjunções, advérbios, etc. |

### V. Relações entre Recursos Expressivos e Efeitos de Sentido

| Descritor | Habilidade |
|---|---|
| D16 | Identificar efeitos de ironia ou humor em textos variados |
| D17 | Reconhecer o efeito de sentido decorrente do uso da pontuação e de outras notações |
| D18 | Reconhecer o efeito de sentido decorrente da escolha de uma determinada palavra ou expressão |
| D19 | Reconhecer o efeito de sentido decorrente da exploração de recursos ortográficos e/ou morfossintáticos |

### VI. Variação Linguística

| Descritor | Habilidade |
|---|---|
| D13 | Identificar as marcas linguísticas que evidenciam o locutor e o interlocutor de um texto |

Total: **21 descritores**. Cada questão da avaliação deve ser mapeada a pelo menos um descritor principal.

```

- [x] **Step 3: Atualizar a seção "Relação com outros conceitos" para incluir link para habilidades-bncc**

Localizar a seção atual `## Relação com outros conceitos`. Adicionar uma linha à lista:

```markdown
- [Habilidades BNCC](./habilidades-bncc.md) — currículo nacional; dialoga com a Matriz SAEB sem substituí-la.
```

- [x] **Step 4: Atualizar a seção "Referências" para apontar para REFERENCIAS.md**

Localizar a seção `## Referências`. Substituir o conteúdo por:

```markdown
- [Matriz de Referência SAEB - Língua Portuguesa - 9º ano (INEP)](../REFERENCIAS.md#saeb--sistema-de-avaliação-da-educação-básica) — referência oficial dos 21 descritores listados acima.
- [BNCC (MEC)](../REFERENCIAS.md#bncc--base-nacional-comum-curricular) — currículo nacional complementar.
```

- [x] **Step 5: Commit**

```bash
git add docs/dominio/conceitos/descritores.md
git commit -m "docs(dominio): adicionar tabela completa D1-D21 da Matriz SAEB LP 9 ano"
```

---

## Task 3: Expandir `conceitos/escala-saepe.md` com estrutura dos 4 níveis

⚠️ **STABILITY-CRITICAL** — conceito teórico do projeto. Pausar antes de aplicar.

**Files:**
- Modify: `docs/dominio/conceitos/escala-saepe.md`

- [x] **Step 1: Ler o arquivo atual**

Ler `docs/dominio/conceitos/escala-saepe.md`.

- [x] **Step 2: Substituir a seção "Faixas de proficiência (referência)"**

Localizar a seção `## Faixas de proficiência (referência)` (atualmente contém apenas o marcador `<!-- precisa de revisão do professor -->`). Substituir por:

```markdown
## Padrões de Desempenho do SAEPE

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: https://avaliacaoemonitoramentopernambuco.caeddigital.net/ + revistas SAEPE 2018/2021 publicadas pelo CAEd · Status: aguarda validação pedagógica. -->

O SAEPE classifica o desempenho dos alunos em **4 padrões** ao longo da escala 0-500:

| Padrão | O que significa pedagogicamente (descrição geral) |
|---|---|
| **Elementar I** | Aluno em estágio inicial de desenvolvimento das habilidades esperadas para a etapa. Demanda intervenção intensiva. |
| **Elementar II** | Aluno em desenvolvimento, ainda abaixo do esperado para a etapa. Demanda atenção pedagógica focada. |
| **Básico** | Aluno demonstra domínio das habilidades mínimas para a etapa. Pode avançar com apoio direcionado. |
| **Desejável** | Aluno demonstra domínio pleno (ou acima) das habilidades esperadas para a etapa. |

### Pontos de corte por etapa e disciplina

Os pontos de corte exatos variam por etapa (5º EF, 9º EF, 3º EM) e disciplina (Língua Portuguesa, Matemática).

**Para Matemática 9º ano EF (extraído do SAEPE 2018):**

| Padrão | Faixa de pontos |
|---|---|
| Elementar I | até 225 |
| Elementar II | 225 a 245 |
| Básico | 245 a 280 |
| Desejável | acima de 280 |

**Para Língua Portuguesa 9º ano EF:**

<!-- precisa de revisão do professor: consultar o relatório oficial SAEPE 2021 (PDF: https://avaliacaoemonitoramentopernambuco.caeddigital.net/resources/arquivos/colecoes/SAEPE_2021/PE2021RPLP.pdf) e preencher os 4 pontos de corte específicos para LP 9º EF. Pontos de corte variam entre disciplinas — não copiar os de Matemática acima. -->

```

- [x] **Step 3: Atualizar a seção "Referências"**

Localizar a seção `## Referências`. Substituir por:

```markdown
- [SAEPE — portal oficial (CAEd)](../REFERENCIAS.md#saepe--sistema-de-avaliação-educacional-de-pernambuco) — fonte oficial de calibração, revistas pedagógicas anuais e relatórios técnicos.
- [Escala de Proficiência SAEB (INEP)](../REFERENCIAS.md#saeb--sistema-de-avaliação-da-educação-básica) — escala-mãe da qual o SAEPE deriva os pontos de corte.
```

- [x] **Step 4: Commit**

```bash
git add docs/dominio/conceitos/escala-saepe.md
git commit -m "docs(dominio): estruturar os 4 padroes de desempenho SAEPE com TODO especifico para LP 9 ano"
```

---

## Task 4: Enriquecer `conceitos/teoria-resposta-item-tri.md` com modelo 3-parâmetros

⚠️ **STABILITY-CRITICAL** — conceito teórico. Pausar antes de aplicar.

**Files:**
- Modify: `docs/dominio/conceitos/teoria-resposta-item-tri.md`

- [x] **Step 1: Ler o arquivo atual**

Ler `docs/dominio/conceitos/teoria-resposta-item-tri.md`.

- [x] **Step 2: Adicionar nova seção "Modelo oficial vs nosso modelo simplificado" antes de "Limites e cuidados"**

Localizar a seção atual `## Limites e cuidados`. Inserir antes dela:

```markdown
## Modelo oficial vs nosso modelo simplificado

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: Nota Técnica TRI ENEM (INEP, 2011) - https://download.inep.gov.br/educacao_basica/enem/nota_tecnica/2011/nota_tecnica_tri.pdf · Status: aguarda validação pedagógica. -->

A TRI oficial usada pelo INEP no ENEM e no SAEB é o **modelo logístico de 3 parâmetros**. Cada item da prova é caracterizado por:

| Parâmetro | Símbolo | O que mede |
|---|---|---|
| Discriminação | **a** | Quão bem a questão distingue alunos com habilidades diferentes. Itens com alta discriminação separam claramente quem domina de quem não domina o conteúdo. |
| Dificuldade | **b** | Nível de habilidade necessário para o aluno ter 50% de probabilidade de acerto (descontando o chute). Itens mais difíceis exigem proficiência mais alta. |
| Acerto ao acaso | **c** | Probabilidade de um aluno acertar a questão por chute. Para questões com 5 alternativas, costuma ficar próximo de 0,20. |

Os parâmetros são **estimados estatisticamente** por calibração com dados reais de aplicação prévia. Uma vez calibrados, eles permitem comparar desempenho entre provas diferentes na mesma escala.

### Diferenças em relação à nossa TRI simplificada

| Parâmetro | TRI oficial (INEP/ENEM/SAEB) | TRI simplificada (este projeto) |
|---|---|---|
| Discriminação (a) | Estimada por calibração estatística | Não estimada — assume itens equivalentes dentro da mesma faixa de dificuldade |
| Dificuldade (b) | Estimada por calibração | Aproximada via TCT (% de erro da turma) |
| Acerto ao acaso (c) | Estimado por calibração | Heurística fixa (penalização de 1/3 dos erros) |
| Comparabilidade entre provas | Sim, dentro da mesma escala calibrada | Não — depende da turma e da prova específica |

A versão simplificada captura a intuição central (penalizar chute, ponderar dificuldade) sem precisar do aparato estatístico de calibração — viável de fazer em planilha, mas com escopo de uso limitado à mesma turma/prova.

```

- [x] **Step 3: Atualizar a seção "Referências"**

Localizar a seção `## Referências`. Substituir por:

```markdown
- [Nota Técnica TRI ENEM (INEP, 2011)](../REFERENCIAS.md#nota-técnica-sobre-tri-no-enem-inep-2011) — documento técnico oficial. Modelo 3-parâmetros descrito na tabela acima.
- [SciELO Brasil](../REFERENCIAS.md#scielo-brasil--scientific-electronic-library-online) — artigos acadêmicos sobre TRI em PT-BR.
- Embretson, S.; Reise, S. — *Item Response Theory for Psychologists*. (Referência clássica EN; ver lista completa em [`REFERENCIAS.md`](../REFERENCIAS.md).)
```

- [x] **Step 4: Commit**

```bash
git add docs/dominio/conceitos/teoria-resposta-item-tri.md
git commit -m "docs(dominio): adicionar modelo 3-parametros da TRI oficial e comparativo com a simplificada"
```

---

## Task 5: Criar `conceitos/habilidades-bncc.md`

⚠️ **STABILITY-CRITICAL** — conceito teórico novo. Pausar antes de aplicar.

**Files:**
- Create: `docs/dominio/conceitos/habilidades-bncc.md`

- [x] **Step 1: Criar arquivo com conteúdo completo**

```markdown
# Habilidades BNCC

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli + time dados_edu

## O que é

A **Base Nacional Comum Curricular (BNCC)** é o documento normativo brasileiro que define as aprendizagens essenciais que todos os alunos devem desenvolver ao longo da Educação Básica. Foi homologada em 20 de dezembro de 2017 e orienta os currículos de todas as redes de ensino (públicas e privadas) do país.

Cada componente curricular (Língua Portuguesa, Matemática, etc.) é organizado em **habilidades** identificadas por códigos padronizados.

## Por que importa para o nosso projeto

A Matriz de Referência SAEB (usada nas avaliações em larga escala como SAEB/SAEPE) e a BNCC **coexistem** — não substituem uma à outra:

- **Matriz SAEB** descreve o que é **avaliado** em larga escala (foco em leitura/interpretação em LP).
- **BNCC** descreve o que deve ser **ensinado** ao longo do ano (foco em todas as áreas: leitura, escrita, oralidade, análise linguística).

O nosso projeto avalia desempenho pela Matriz SAEB (porque o instrumento é compatível), mas ao planejar [intervenções](./sequencias-didaticas.md) e [agrupar alunos](../processos/agrupamento-de-alunos.md), o professor consulta a BNCC para garantir cobertura curricular completa.

## Como funciona — codificação das habilidades

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: https://basenacionalcomum.mec.gov.br/images/BNCC_20dez_site.pdf · Status: aguarda validação pedagógica. -->

Cada habilidade BNCC tem um código no formato:

```
EF<ANO><DISCIPLINA><NÚMERO>
```

Para Língua Portuguesa do 9º ano:

- `EF` = Ensino Fundamental
- `09` = 9º ano (quando a habilidade é específica de um ano)
- `LP` = Língua Portuguesa
- `01, 02, 03...` = número sequencial

Exemplo: `EF09LP01` = primeira habilidade específica do 9º ano de Língua Portuguesa.

Há também códigos compartilhados:

- `EF89LP...` = habilidade compartilhada entre 8º e 9º anos.
- `EF69LP...` = habilidade do 6º ao 9º ano (todos os anos finais do EF).

## Exemplos de habilidades EF09LP

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: BNCC PDF oficial - https://basenacionalcomum.mec.gov.br/images/BNCC_20dez_site.pdf · Status: aguarda validação pedagógica. -->

Amostra de habilidades específicas do 9º ano (transcrição literal de fonte oficial):

- **EF09LP01** — Analisar o fenômeno da disseminação de notícias falsas nas redes sociais e desenvolver estratégias para reconhecê-las, a partir da verificação/avaliação do veículo, fonte, data e local da publicação, autoria, URL, da análise da formatação, da comparação de diferentes fontes, da consulta a sites de curadoria que atestam a fidedignidade do relato dos fatos e denunciam boatos etc.

- **EF09LP03** — Produzir artigos de opinião, tendo em vista o contexto de produção dado, assumindo posição diante de tema polêmico, argumentando de acordo com a estrutura própria desse tipo de texto e utilizando diferentes tipos de argumentos — de autoridade, comprovação, exemplificação, princípio etc.

- **EF09LP07** — Comparar o uso de regência verbal e regência nominal na norma-padrão com seu uso no português brasileiro coloquial oral.

- **EF09LP08** — Identificar, em textos lidos e em produções próprias, a relação que conjunções (e locuções conjuntivas) coordenativas e subordinativas estabelecem entre as orações que conectam.

A lista completa está no documento oficial da BNCC (link em [`REFERENCIAS.md`](../REFERENCIAS.md#bncc--base-nacional-comum-curricular)).

## Relação com a Matriz SAEB

<!-- precisa de revisão do professor: descrever como você usa BNCC e Matriz SAEB em conjunto na prática (ex.: SAEB orienta o que medimos; BNCC orienta o que ensinamos antes/depois). Exemplo concreto de uma sequência didática que cobre uma habilidade BNCC e desenvolve um descritor SAEB ao mesmo tempo. -->

## Limites e cuidados

- BNCC define competências e habilidades por ano, mas redes de ensino têm liberdade para organizar internamente. Verifique se a rede da escola adota a BNCC pura ou um currículo derivado.
- Nem toda habilidade BNCC casa 1:1 com descritor SAEB — alguns descritores cobrem várias habilidades; algumas habilidades não são avaliáveis por prova escrita.

## Relação com outros conceitos

- [Descritores SAEB](./descritores.md) — referencial de avaliação, complementar à BNCC.
- [Sequências didáticas](./sequencias-didaticas.md) — onde BNCC vira plano de aula.
- [Avaliação diagnóstica](./avaliacao-diagnostica.md) — fecha o loop: identificar lacunas por descritor SAEB e replanejar cobertura por habilidade BNCC.

## Referências

- [BNCC — portal oficial e PDF completo](../REFERENCIAS.md#bncc--base-nacional-comum-curricular).
- [MEC — Ministério da Educação](../REFERENCIAS.md#mec--ministério-da-educação).
```

- [x] **Step 2: Commit**

```bash
git add docs/dominio/conceitos/habilidades-bncc.md
git commit -m "docs(dominio): adicionar conceito habilidades-bncc com codificacao EF09LP e exemplos oficiais"
```

---

## Task 6: Atualizar slice `dominio-descritores.md`

**Files:**
- Modify: `docs/llm/slices/dominio-descritores.md`

- [x] **Step 1: Ler o slice atual**

Ler `docs/llm/slices/dominio-descritores.md`.

- [x] **Step 2: Substituir conteúdo**

Substituir o arquivo inteiro por:

```markdown
---
id: dominio-descritores
title: Descritores
when-to-load: ["descritor", "matriz SAEB", "BNCC", "habilidade", "D1", "D5", "tópico", "leitura"]
owners-paths: ["docs/dominio/conceitos/descritores.md", "docs/dominio/conceitos/habilidades-bncc.md"]
last-verified: 2026-05-18
version: 2
---

## Resumo

- Descritor = habilidade curricular avaliada por uma questão.
- Codificação (D1, D5…) vem da Matriz SAEB (referência principal do projeto).
- Análise por descritor é o eixo da intervenção pedagógica do projeto.
- BNCC coexiste com Matriz SAEB — define o que ensinar, enquanto a Matriz define o que avaliar.

## Catálogo SAEB LP 9º ano (21 descritores, 6 tópicos)

| Tópico | Descritores |
|---|---|
| I. Procedimentos de Leitura | D1, D3, D4, D6, D14 |
| II. Implicações do Suporte/Gênero/Enunciador | D5, D12 |
| III. Relação entre Textos | D20, D21 |
| IV. Coerência e Coesão | D2, D7, D8, D9, D10, D11, D15 |
| V. Recursos Expressivos e Efeitos de Sentido | D16, D17, D18, D19 |
| VI. Variação Linguística | D13 |

Total: 21. Lista completa com habilidades em `docs/dominio/conceitos/descritores.md`.

## Fatos-chave

- Uma questão pode tocar múltiplos descritores; tipicamente uma como principal.
- Descritor com baixo desempenho = "descritor crítico" = prioridade de intervenção.
- BNCC usa codificação `EFXXLPNN` (ex.: EF09LP01) — não confundir com `Dnn` do SAEB.

## Cuidados

- Vinculação questão↔descritor é subjetiva.
- Matriz SAEB ≠ BNCC; pode haver descritor SAEB sem habilidade BNCC equivalente e vice-versa.

## Para aprofundar

- [`docs/dominio/conceitos/descritores.md`](../../dominio/conceitos/descritores.md) — tabela nominal completa D1-D21.
- [`docs/dominio/conceitos/habilidades-bncc.md`](../../dominio/conceitos/habilidades-bncc.md) — codificação BNCC e exemplos.
- [`docs/dominio/REFERENCIAS.md`](../../dominio/REFERENCIAS.md) — fontes oficiais (INEP, BNCC).
```

- [x] **Step 3: Commit**

```bash
git add docs/llm/slices/dominio-descritores.md
git commit -m "docs(llm): atualizar slice dominio-descritores com catalogo D1-D21 e BNCC"
```

---

## Task 7: Atualizar slice `dominio-tri-simplificado.md` e criar `dominio-habilidades-bncc.md`

**Files:**
- Modify: `docs/llm/slices/dominio-tri-simplificado.md`
- Create: `docs/llm/slices/dominio-habilidades-bncc.md`

- [x] **Step 1: Atualizar `dominio-tri-simplificado.md`**

Ler o arquivo atual e substituir por:

```markdown
---
id: dominio-tri-simplificado
title: TRI Simplificada do Projeto
when-to-load: ["TRI", "penalização", "chute", "ponderação", "nota ponderada", "SAEPE", "fórmula", "3 parâmetros", "discriminação", "dificuldade", "acerto ao acaso"]
owners-paths: ["docs/dominio/conceitos/teoria-resposta-item-tri.md", "docs/dominio/formulas/tri-simplificado.md", "docs/dominio/conceitos/escala-saepe.md"]
last-verified: 2026-05-18
version: 2
---

## Resumo

- Versão simplificada da TRI, não a oficial do ENEM.
- TRI oficial usa modelo logístico de 3 parâmetros (a/b/c) estimados por calibração estatística.
- Nossa versão simplificada combina TCT + penalização de chute + ponderação por dificuldade.
- Saída final convertida para escala SAEPE (0-500).

## Pipeline da simplificada

```
Acertos brutos por dificuldade (F/M/D)
   ↓ penalização: (total - erros) - erros/3
Pontos corrigidos AT, AU, AV
   ↓ ajuste especial das fáceis
AW (se acerto em fáceis < 50%, suaviza AV)
   ↓ ponderação: AT*1 + AU*1,5 + AW*2
AX (nota real ponderada)
   ↓ conversão linear
AZ (0-500)
```

## Modelo oficial vs simplificado (resumo)

| Parâmetro | Oficial (INEP) | Nosso |
|---|---|---|
| a (discriminação) | calibração | não estimado |
| b (dificuldade) | calibração | aproximado via TCT (% erro) |
| c (acerto ao acaso) | calibração | heurística 1/3 |

## Fatos-chave

- Pesos da simplificada: fácil = 1, médio = 1,5, difícil = 2.
- Hardcoded `13` (total fáceis na prova de referência) → vira parâmetro quando virar sistema.
- Comparabilidade entre provas só com a TRI oficial (calibrada).

## Cuidados

- ⚠️ Stability-critical: mudar fórmula altera análise de centenas de alunos.
- Saída SAEPE é aproximação; comunicar isso em relatórios.

## Para aprofundar

- [`docs/dominio/formulas/tri-simplificado.md`](../../dominio/formulas/tri-simplificado.md) — fórmulas completas.
- [`docs/dominio/conceitos/teoria-resposta-item-tri.md`](../../dominio/conceitos/teoria-resposta-item-tri.md) — teoria + tabela oficial vs simplificado.
- [`docs/dominio/conceitos/escala-saepe.md`](../../dominio/conceitos/escala-saepe.md) — saída final.
- [`docs/dominio/REFERENCIAS.md`](../../dominio/REFERENCIAS.md#nota-técnica-sobre-tri-no-enem-inep-2011) — Nota Técnica TRI ENEM (INEP).
```

- [x] **Step 2: Criar `dominio-habilidades-bncc.md`**

```markdown
---
id: dominio-habilidades-bncc
title: Habilidades BNCC
when-to-load: ["BNCC", "habilidade", "currículo", "EF09LP", "EF89LP", "competência"]
owners-paths: ["docs/dominio/conceitos/habilidades-bncc.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- BNCC = currículo nacional homologado em 2017. Define o que ensinar.
- Codificação: `EF<ANO><DISCIPLINA><NÚMERO>` (ex.: `EF09LP01`).
- Coexiste com Matriz SAEB — não substitui.

## Codificação

- `EF` = Ensino Fundamental.
- `09` = 9º ano (específico) · `89` = compartilhado 8º+9º · `69` = 6º ao 9º.
- `LP` = Língua Portuguesa · `MA` = Matemática · etc.
- `01, 02…` = sequencial.

## Fatos-chave

- BNCC ≠ Matriz SAEB. Matriz avalia, BNCC ensina.
- Nem toda habilidade BNCC é avaliável por prova escrita (algumas são orais, de produção, etc.).
- Redes podem adotar BNCC pura ou currículo derivado — verificar com a escola.

## Cuidados

- Não confundir `Dnn` (SAEB) com `EFXXLPNN` (BNCC).
- Habilidade BNCC e descritor SAEB podem cobrir áreas parcialmente sobrepostas — mapeamento exige cuidado.

## Para aprofundar

- [`docs/dominio/conceitos/habilidades-bncc.md`](../../dominio/conceitos/habilidades-bncc.md) — explicação completa + exemplos transcritos.
- [`docs/dominio/REFERENCIAS.md`](../../dominio/REFERENCIAS.md#bncc--base-nacional-comum-curricular) — fonte oficial BNCC.
```

- [x] **Step 3: Commit**

```bash
git add docs/llm/slices/dominio-tri-simplificado.md docs/llm/slices/dominio-habilidades-bncc.md
git commit -m "docs(llm): atualizar slice TRI com modelo 3-parametros e adicionar slice habilidades-bncc"
```

---

## Task 8: Atualizar `docs/llm/MAP.md` com trigger da nova slice

**Files:**
- Modify: `docs/llm/MAP.md`

- [x] **Step 1: Adicionar linha à tabela "Quando carregar qual slice"**

Ler `docs/llm/MAP.md`. Localizar a tabela `| Se o tópico do usuário contém… | Carregue |`.

Adicionar linha após a linha "descritor, matriz SAEB, BNCC...":

```markdown
| "BNCC", "habilidade", "EF09LP", "EF89LP", "currículo"   | `slices/dominio-habilidades-bncc.md` |
```

E remover "BNCC" da linha de descritores (passou a ter slice dedicada):

Linha atual:
```markdown
| "descritor", "matriz SAEB", "BNCC", "habilidade"       | `slices/dominio-descritores.md`     |
```

Substituir por:
```markdown
| "descritor", "matriz SAEB", "tópico", "D1", "D5"        | `slices/dominio-descritores.md`     |
```

- [x] **Step 2: Commit**

```bash
git add docs/llm/MAP.md
git commit -m "docs(llm): adicionar trigger para slice habilidades-bncc no MAP"
```

---

## Task 9: Verificação cruzada

**Files:**
- Read-only across the changed files.

- [ ] **Step 1: Verificar marcadores EXTRAÍDO bem-formados**

```bash
grep -rn "EXTRAÍDO de fonte oficial" docs/dominio/
```

Esperado: cada match contém data (`2026-05-18`), URL completa começando com `https://`, e `Status: aguarda validação pedagógica`.

- [ ] **Step 2: Verificar contagem de descritores**

```bash
grep -E "^\| D[0-9]+" docs/dominio/conceitos/descritores.md | wc -l
```

Esperado: 21.

- [ ] **Step 3: Verificar que os 4 padrões SAEPE estão nomeados**

```bash
grep -E "Elementar I|Elementar II|Básico|Desejável" docs/dominio/conceitos/escala-saepe.md | wc -l
```

Esperado: ≥4 (cada nome aparece pelo menos uma vez).

- [ ] **Step 4: Verificar links cruzados para REFERENCIAS.md**

```bash
grep -l "REFERENCIAS.md" docs/dominio/conceitos/descritores.md docs/dominio/conceitos/escala-saepe.md docs/dominio/conceitos/teoria-resposta-item-tri.md docs/dominio/conceitos/habilidades-bncc.md
```

Esperado: 4 arquivos (todos os tocados).

- [ ] **Step 5: Verificar slices atualizadas**

```bash
for f in docs/llm/slices/dominio-descritores.md docs/llm/slices/dominio-tri-simplificado.md docs/llm/slices/dominio-habilidades-bncc.md; do
  echo "=== $f ==="
  head -10 "$f"
done
```

Esperado: cada slice tem `last-verified: 2026-05-18` e `version: 1` ou `2` conforme aplicável.

- [ ] **Step 6: Verificar MAP.md tem o novo trigger**

```bash
grep "habilidades-bncc" docs/llm/MAP.md
```

Esperado: pelo menos uma menção.

- [ ] **Step 7: Commit (apenas se houve correção)**

Se algum dos passos exigiu fix:

```bash
git add docs/
git commit -m "docs: corrigir consistencia apos extracao de fontes oficiais"
```

Caso contrário, pular este step.

---

## Self-Review do Plan

**Spec coverage:**

| Critério da spec | Task que cobre |
|---|---|
| Documentar nova convenção de marcador | Task 1 |
| Tabela D1-D21 em descritores.md | Task 2 |
| Estrutura dos 4 níveis SAEPE em escala-saepe.md | Task 3 |
| Modelo 3-parâmetros em teoria-resposta-item-tri.md | Task 4 |
| Conceito novo habilidades-bncc.md | Task 5 |
| Slice dominio-descritores atualizada | Task 6 |
| Slice dominio-tri-simplificado atualizada + nova slice BNCC | Task 7 |
| MAP.md com novo trigger | Task 8 |
| Verificação cruzada | Task 9 |

**Placeholder scan:** nenhum `TBD` ou "preencher depois" sem contexto. Os `<!-- precisa de revisão do professor: ... -->` que persistem têm pergunta concreta (pontos de corte SAEPE LP 9º; relação BNCC↔SAEB na prática).

**Type consistency:** todos os caminhos coerentes; nomes de slices (`dominio-descritores`, `dominio-tri-simplificado`, `dominio-habilidades-bncc`) idênticos em todas as menções (frontmatter, MAP, links).

**Ordem:** Task 1 (convenção) precede Tasks 2-5 (uso da convenção). Tasks 6-8 (LLM) dependem do domínio atualizado. Task 9 por último.
