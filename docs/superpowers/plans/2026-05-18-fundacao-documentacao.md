# Fundação de Documentação Implementation Plan

> **Nota histórica:** este plano menciona um arquivo `post_alikaeli` que existia na raiz do repositório durante a execução. Esse arquivo era o texto bruto do prof. Alikaeli descrevendo seu trabalho — serviu de fonte para popular `docs/dominio/` e foi removido após a extração por ser informal e fora do padrão. Todo conteúdo essencial vive hoje em `docs/dominio/`.
>
> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Estabelecer a fundação de documentação do `dados_edu` (greenfield, contexto educacional) cobrindo domínio pedagógico, slices LLM-first, esqueleto dev, adaptação completa do `.claude/` e novo README.

**Architecture:** Três camadas — `docs/dominio/` (fonte canônica humana), `docs/llm/` (slices compactos sob demanda) e `docs/dev/` (documentação técnica leve). `.claude/` reescrito para R-P-I-V no contexto educacional. `README.md` vira vitrine + ponteiros.

**Tech Stack:** Markdown puro. Sem geradores, sem CI de docs. Convenção: Conventional Commits. Datas em ISO.

**Spec:** [`docs/superpowers/specs/2026-05-18-fundacao-documentacao-design.md`](../specs/2026-05-18-fundacao-documentacao-design.md)

**Ordem de execução:** esqueleto/INDEX → dominio → llm → dev → .claude → README → verificação. Cada bloco resolve links para o próximo.

**Convenção de "última atualização":** Todo doc top-level recebe header `> Última atualização: 2026-05-18 · Mantenedor: time dados_edu`.

**Stability-critical (pausa antes de tocar):** qualquer arquivo em `docs/dominio/formulas/`, `docs/dominio/conceitos/teoria-*`, e o arquivo `docs/dominio/GLOSSARIO.md` quando a edição alterar definição (não adição de termo novo).

---

## Task 1: Criar `docs/INDEX.md` e esqueleto de pastas

**Files:**
- Create: `docs/INDEX.md`
- Create: `docs/dominio/` (pasta vazia por enquanto, criada implicitamente nas tasks seguintes)
- Create: `docs/dev/`
- Create: `docs/llm/slices/`
- Create: `docs/llm/conventions/`
- Create: `docs/superpowers/specs/` (já existe via spec)
- Create: `docs/superpowers/plans/` (já existe via este plano)

- [x] **Step 1: Criar `docs/INDEX.md` com mapa-mor**

Escrever `docs/INDEX.md` com este conteúdo exato:

```markdown
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
```

- [x] **Step 2: Commit**

```bash
git add docs/INDEX.md
git commit -m "docs: adicionar INDEX.md como mapa-mor da documentacao"
```

---

## Task 2: Criar `docs/dominio/README.md` e GLOSSARIO

**Files:**
- Create: `docs/dominio/README.md`
- Create: `docs/dominio/GLOSSARIO.md`

- [x] **Step 1: Criar `docs/dominio/README.md` (onboarding do professor)**

Conteúdo exato:

````markdown
# Domínio Pedagógico

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli + time dados_edu

Esta pasta é a **fonte canônica** do conhecimento pedagógico do projeto. Tudo que dizemos sobre avaliação, descritores, TCT, TRI, intervenção pedagógica e escala SAEPE começa aqui.

## Estrutura

| Subpasta       | O que vai aqui                                                                       |
|----------------|--------------------------------------------------------------------------------------|
| [`conceitos/`](./conceitos/)   | Teoria. Um arquivo por conceito (TCT, TRI, descritores, níveis de leitura…).        |
| [`processos/`](./processos/)   | Como o trabalho acontece hoje (ciclo avaliativo, coleta, análise, agrupamento).     |
| [`formulas/`](./formulas/)     | Matemática real do projeto (fórmulas de TRI simplificado, planilhas, exemplos).     |
| [`_templates/`](./_templates/) | Esqueletos para o professor preencher conceitos/processos novos sem precisar criar markdown do zero. |
| [`GLOSSARIO.md`](./GLOSSARIO.md) | Vocabulário PT-BR com definições completas e exemplos de uso.                     |

## Para o professor: como contribuir

Você é a fonte da verdade do conteúdo pedagógico deste projeto. Aqui está como contribuir sem precisar virar dev:

### Adicionar um conceito novo

1. Abra [`_templates/novo-conceito.md`](./_templates/novo-conceito.md) e copie o conteúdo.
2. Crie um arquivo novo em `conceitos/` com nome em minúsculas separado por hífen: `meu-conceito-novo.md`.
3. Cole o template no arquivo e preencha cada seção.
4. Se não souber editar diretamente, peça ao Claude: "preenche esse template com base no que vou te explicar sobre X" e cole sua explicação.

### Adicionar um processo novo

Igual ao conceito, mas usando [`_templates/novo-processo.md`](./_templates/novo-processo.md) e salvando em `processos/`.

### Atualizar uma fórmula

Fórmulas vivem em `formulas/`. Se você descobrir que uma fórmula está errada ou tem uma versão melhor:

1. Abra o arquivo da fórmula.
2. Edite o bloco da fórmula explicando **por que** mudou (essa parte é crítica).
3. Marque o cabeçalho com a data nova.
4. Se possível, peça revisão a outro professor da área antes de confirmar.

> ⚠️ Mexer em fórmula é considerado "área sensível" no nosso workflow. Quem implementar mudança aqui pausa e pede confirmação antes de aplicar — é proteção contra erro silencioso.

### Adicionar termo ao glossário

Abra [`GLOSSARIO.md`](./GLOSSARIO.md) e adicione na ordem alfabética. Cada termo tem três campos:

- **Definição** — uma frase clara.
- **Exemplo de uso** — uma frase em contexto.
- **Referência** (opcional) — de onde a definição vem (BNCC, INEP, Vygotsky, etc.).

## Para o dev: como consumir

Tudo em `dominio/` é texto narrativo, otimizado para humanos. Quando o agente LLM precisa do mesmo conteúdo de forma compacta, ele lê os equivalentes em [`docs/llm/slices/`](../llm/slices/). Não duplique manualmente — rode `/verify-and-doc` para sincronizar.
````

- [x] **Step 2: Criar `docs/dominio/GLOSSARIO.md` (seed inicial)**

Conteúdo exato (extraído do post + termos canônicos do domínio):

```markdown
# Glossário (Domínio Pedagógico)

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli + time dados_edu

Vocabulário PT-BR do projeto. Ordem alfabética. Cada termo tem definição, exemplo de uso e (quando aplicável) referência.

---

## Agrupamento por nível

**Definição:** Organização dos alunos em grupos baseada no perfil de desempenho identificado nas avaliações, para aplicar intervenções específicas.

**Exemplo de uso:** "Depois da análise por descritor, fiz o agrupamento por nível e cada grupo recebeu uma sequência didática diferente."

---

## Avaliação diagnóstica

**Definição:** Instrumento aplicado para identificar o nível de conhecimento atual dos alunos em determinadas habilidades, antes de iniciar uma intervenção.

**Exemplo de uso:** "Apliquei a avaliação diagnóstica de leitura na primeira semana para mapear quais descritores precisam de mais trabalho."

---

## Descritor

**Definição:** Habilidade curricular específica que uma questão avalia. Cada descritor é uma referência codificada da BNCC ou da matriz curricular adotada (ex.: D1, D2 da Matriz SAEB).

**Exemplo de uso:** "O descritor D5 mede a capacidade de localizar informação explícita no texto."

**Referência:** Matriz de Referência SAEB / BNCC.

---

## Escala SAEPE

**Definição:** Escala de proficiência de 0 a 500 utilizada para reportar desempenho em avaliações em larga escala em Pernambuco. O sistema do projeto converte notas internas para essa escala como saída final.

**Exemplo de uso:** "O aluno teve 320 na escala SAEPE — está em nível básico para o 9º ano."

**Referência:** Sistema de Avaliação Educacional de Pernambuco.

---

## Gênero textual

**Definição:** Tipo de texto definido por sua função social e estrutura (conto, notícia, artigo de opinião, etc.). A curadoria de gêneros para sequências didáticas considera o currículo e os descritores a desenvolver.

**Exemplo de uso:** "Para trabalhar localização de informação, escolhi gêneros notícia e verbete de enciclopédia."

---

## Intervenção pedagógica

**Definição:** Conjunto de atividades planejadas para superar dificuldades identificadas em uma avaliação. No projeto, cada grupo de alunos recebe intervenção específica.

**Exemplo de uso:** "A intervenção para o grupo de leitor iniciante focou em decodificação antes de avançar para inferência."

---

## Nível de leitura

**Definição:** Classificação do aluno em estágios de proficiência leitora (leitor iniciante, em desenvolvimento, proficiente, etc.). Usado para agrupar alunos em intervenções específicas.

**Exemplo de uso:** "Identificamos três níveis de leitura na turma; cada um recebeu uma sequência didática diferente."

---

## Penalização de acerto ao acaso

**Definição:** Ajuste matemático aplicado ao escore para reduzir a contribuição de questões cujo acerto pode ter sido resultado de chute. Inspirado em TRI mas implementado de forma simplificada via planilha.

**Exemplo de uso:** "Aplico penalização ao acaso nas questões fáceis para que o resultado reflita mais o conhecimento real."

**Referência:** Veja [`formulas/tri-simplificado.md`](./formulas/tri-simplificado.md) para a fórmula exata usada no projeto.

---

## Sequência didática

**Definição:** Conjunto encadeado de atividades planejadas para desenvolver um conjunto específico de habilidades em determinado tempo. No projeto, cada perfil de aluno recebe uma sequência adequada.

**Exemplo de uso:** "Montei uma sequência didática de quatro aulas focada em inferência."

**Referência:** Dolz, Noverraz e Schneuwly (2004).

---

## Simulado

**Definição:** Avaliação aplicada simulando provas externas (ENEM, SAEB, SAEPE). Serve tanto para diagnóstico quanto para treinamento.

**Exemplo de uso:** "O simulado de fim de bimestre testou descritores de leitura e gramática."

---

## TCT (Teoria Clássica dos Testes)

**Definição:** Modelo psicométrico que avalia desempenho por proporção de acerto. No projeto, classifica questões em fácil/médio/difícil:
- erro > 50% → difícil
- 50% < acerto ≤ 70% → médio
- acerto > 70% → fácil

**Exemplo de uso:** "Pela TCT, três questões da prova foram classificadas como difíceis — vão receber atenção na revisão."

**Referência:** Veja [`conceitos/teoria-classica-testes-tct.md`](./conceitos/teoria-classica-testes-tct.md).

---

## TRI (Teoria de Resposta ao Item)

**Definição:** Modelo psicométrico que avalia o desempenho considerando a dificuldade de cada questão e padrões de chute. Usado em ENEM e SAEB. No projeto, aplicamos uma versão simplificada baseada em TCT + penalizações.

**Exemplo de uso:** "A TRI simplificada que uso pondera fáceis, médias e difíceis com pesos diferentes."

**Referência:** Veja [`conceitos/teoria-resposta-item-tri.md`](./conceitos/teoria-resposta-item-tri.md) e [`formulas/tri-simplificado.md`](./formulas/tri-simplificado.md).
```

- [x] **Step 3: Commit**

```bash
git add docs/dominio/README.md docs/dominio/GLOSSARIO.md
git commit -m "docs(dominio): adicionar README e GLOSSARIO inicial"
```

---

## Task 3: Criar templates de domínio

**Files:**
- Create: `docs/dominio/_templates/novo-conceito.md`
- Create: `docs/dominio/_templates/novo-processo.md`
- Create: `docs/dominio/_templates/nova-formula.md`

- [x] **Step 1: Criar `_templates/novo-conceito.md`**

Conteúdo exato:

```markdown
# <Nome do conceito>

> Última atualização: AAAA-MM-DD · Mantenedor: <quem é responsável por esse conceito>

<!--
COMO USAR ESTE TEMPLATE:
1. Substitua "<Nome do conceito>" pelo nome real (ex.: "Teoria Clássica dos Testes").
2. Preencha cada seção abaixo. Apague as seções que não fizerem sentido para este conceito.
3. Apague esses comentários antes de salvar.
-->

## O que é

<!-- Uma a duas frases que definem o conceito de forma simples. Imagine explicando para um colega de outra área. -->

## Por que importa para o nosso projeto

<!-- Em que momento do projeto esse conceito aparece? Por que precisamos dele? -->

## Como funciona

<!-- Explicação mais profunda. Use subseções (### …) se precisar. -->

## Exemplo concreto

<!-- Um exemplo real, idealmente com números ou um caso de turma real (anonimizado). -->

## Limites e cuidados

<!-- O que esse conceito NÃO faz? Onde alguém pode se confundir? -->

## Relação com outros conceitos

<!-- Liste conceitos relacionados — referencie com link relativo a docs/dominio/conceitos/. -->

- [<conceito relacionado>](./<arquivo>.md)

## Referências

<!-- Livros, artigos, documentos oficiais (BNCC, INEP, SAEPE). Cite com cuidado. -->

- 
```

- [x] **Step 2: Criar `_templates/novo-processo.md`**

Conteúdo exato:

```markdown
# Processo: <Nome do processo>

> Última atualização: AAAA-MM-DD · Mantenedor: <quem executa esse processo hoje>

<!--
COMO USAR ESTE TEMPLATE:
1. Substitua "<Nome do processo>" pelo nome real (ex.: "Coleta de dados de simulado").
2. Preencha cada seção. Apague o que não se aplicar.
3. Apague esses comentários antes de salvar.
-->

## Objetivo do processo

<!-- O que esse processo entrega? Qual o resultado esperado no final? -->

## Quando ele acontece

<!-- Em que momento do ciclo letivo? Qual a periodicidade? -->

## Pré-requisitos

<!-- O que precisa estar pronto antes de começar? -->

## Passo a passo (como é feito hoje)

<!-- Numere as etapas. Seja específico — quem faz, com qual ferramenta, gerando qual artefato. -->

1. 
2. 
3. 

## Artefatos produzidos

<!-- O que sai desse processo? Planilhas, relatórios, agrupamentos, decisões? -->

## Pontos de dor

<!-- O que dói hoje nesse processo? O que toma muito tempo, o que erra com frequência? Isso é ouro para priorizar automação. -->

## O que seria ideal

<!-- Se tivéssemos o sistema pronto, como esse processo seria? -->

## Conceitos relacionados

- [<conceito>](../conceitos/<arquivo>.md)
```

- [x] **Step 3: Criar `_templates/nova-formula.md`**

Conteúdo exato:

````markdown
# Fórmula: <Nome>

> Última atualização: AAAA-MM-DD · Mantenedor: <responsável pela fórmula>

<!--
COMO USAR ESTE TEMPLATE:
1. Substitua "<Nome>" pelo nome da fórmula (ex.: "Penalização de chute em questões fáceis").
2. Preencha cada seção.
3. Apague esses comentários antes de salvar.

⚠️ FÓRMULAS SÃO ÁREA SENSÍVEL. Antes de mudar uma fórmula existente, peça revisão a outro professor.
-->

## Para que serve

<!-- Em uma frase: o que essa fórmula calcula e por quê. -->

## Fórmula

```
<colar aqui a fórmula exata, em sintaxe de planilha (Excel/Google Sheets) ou notação matemática>
```

## Variáveis

| Símbolo / Coluna | Significado                                    | Origem                   |
|------------------|------------------------------------------------|--------------------------|
| `AN3`            | <ex.: número de erros em questões fáceis>      | <ex.: linha da planilha> |
| ...              |                                                |                          |

## Exemplo numérico

<!-- Mostre a fórmula aplicada a valores concretos. -->

```
<exemplo passo-a-passo>
```

## Por que essa fórmula foi escolhida

<!-- Que problema ela resolve? Por que não outra abordagem? -->

## Limites conhecidos

<!-- Casos em que a fórmula não é ideal. Quando substituir por algo mais robusto. -->

## Referências

<!-- Modelos psicométricos, papers, planilhas originais. -->

- 
````

- [x] **Step 4: Commit**

```bash
git add docs/dominio/_templates/
git commit -m "docs(dominio): adicionar templates de conceito, processo e formula"
```

---

## Task 4: Popular `dominio/conceitos/` a partir do post

**Files:**
- Create: `docs/dominio/conceitos/avaliacao-diagnostica.md`
- Create: `docs/dominio/conceitos/descritores.md`
- Create: `docs/dominio/conceitos/teoria-classica-testes-tct.md`
- Create: `docs/dominio/conceitos/teoria-resposta-item-tri.md`
- Create: `docs/dominio/conceitos/niveis-de-leitura.md`
- Create: `docs/dominio/conceitos/sequencias-didaticas.md`
- Create: `docs/dominio/conceitos/escala-saepe.md`

Para cada arquivo abaixo, usar o template `_templates/novo-conceito.md` como esqueleto e preencher com base no conteúdo do `post_alikaeli` (já lido em contexto). Onde o post não detalha, deixar a seção curta e marcar com `<!-- precisa de revisão do professor -->` para que o Alikaeli expanda.

- [x] **Step 1: Criar `conceitos/teoria-classica-testes-tct.md`**

Conteúdo:

```markdown
# Teoria Clássica dos Testes (TCT)

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

A Teoria Clássica dos Testes (TCT) é um modelo psicométrico que avalia o desempenho dos alunos analisando a proporção de acertos e erros em cada questão de uma avaliação. É o modelo mais antigo e mais simples, base para muitos sistemas de avaliação interna.

## Por que importa para o nosso projeto

A TCT é o ponto de partida da nossa análise. Ela permite, com pouco esforço, classificar as questões da prova em fácil, médio e difícil — e isso direciona como vamos ponderar o desempenho de cada aluno e onde concentrar a intervenção pedagógica.

## Como funciona

A TCT classifica questões pela proporção de acertos da turma:

| Faixa de acertos                | Classificação |
|---------------------------------|---------------|
| Acima de 70%                    | Fácil         |
| Acima de 50% e até 70%          | Médio         |
| 50% ou menos (erro > 50%)       | Difícil       |

Essa classificação alimenta o cálculo da nota ponderada, em que questões difíceis valem mais que fáceis. Veja [`formulas/tri-simplificado.md`](../formulas/tri-simplificado.md).

## Exemplo concreto

Em uma turma de 30 alunos, a questão 1 teve 25 acertos (83%) — classificada como **fácil**. A questão 7 teve 12 acertos (40%) — classificada como **difícil**.

## Limites e cuidados

- A TCT depende da turma: a mesma questão pode ser "fácil" numa turma e "difícil" em outra. Isso impede comparação direta entre turmas.
- A TCT não considera padrão de chute. Por isso, no projeto, combinamos TCT com uma versão simplificada de TRI.

## Relação com outros conceitos

- [Teoria de Resposta ao Item (TRI)](./teoria-resposta-item-tri.md) — modelo mais sofisticado, usado em ENEM/SAEB.
- [Descritores](./descritores.md) — cada questão tem um descritor, e a análise TCT pode ser feita por descritor.

## Referências

- Pasquali, L. *Psicometria: teoria dos testes na psicologia e na educação*.
```

- [x] **Step 2: Criar `conceitos/teoria-resposta-item-tri.md`**

Conteúdo:

```markdown
# Teoria de Resposta ao Item (TRI) — versão simplificada

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

A Teoria de Resposta ao Item (TRI) é um modelo psicométrico avançado que estima a habilidade do aluno considerando: a dificuldade de cada questão, o poder de discriminação da questão e a probabilidade de acerto por chute. É o modelo usado oficialmente no ENEM e em avaliações em larga escala como SAEB e SAEPE.

A TRI **completa** exige conhecimento estatístico e ferramentas específicas. No projeto, usamos uma **versão simplificada** que captura a intuição central — penalizar o chute e ponderar a dificuldade — sem o aparato matemático completo.

## Por que importa para o nosso projeto

A TRI simplificada nos permite gerar uma nota que reflete mais o conhecimento real do aluno, e não só "quantas questões ele acertou". Isso é especialmente útil quando comparamos desempenho em provas com mesma matriz mas questões diferentes.

## Como funciona

Versão simplificada usada no projeto, em três etapas:

1. **Classificar questões por dificuldade** via [TCT](./teoria-classica-testes-tct.md).
2. **Penalizar acerto ao acaso** — subtrair uma fração estimada de chute em cada categoria de dificuldade.
3. **Ponderar a nota** dando peso maior a questões mais difíceis.

A fórmula exata está em [`formulas/tri-simplificado.md`](../formulas/tri-simplificado.md).

## Exemplo concreto

Um aluno acerta 8 questões fáceis, 4 médias e 2 difíceis (total 14). Pelo escore bruto seria 14. Com a TRI simplificada, ele recebe uma nota ponderada que reconhece o esforço maior nas difíceis e desconta possível chute nas fáceis. O resultado é convertido para a [escala SAEPE](./escala-saepe.md) (0 a 500).

## Limites e cuidados

- Não substitui a TRI oficial; é uma heurística operacional.
- Os pesos (1, 1.5, 2) e os fatores de penalização foram calibrados empiricamente pelo professor. Mudá-los exige revisão.
- A versão simplificada não estima discriminação de itens — assume que todas as questões da mesma faixa de dificuldade são equivalentes.

## Relação com outros conceitos

- [TCT](./teoria-classica-testes-tct.md) — entrada da TRI simplificada.
- [Escala SAEPE](./escala-saepe.md) — saída final.
- [Penalização de acerto ao acaso](../GLOSSARIO.md#penalização-de-acerto-ao-acaso) — entrada do ajuste.

## Referências

- INEP. *Nota técnica sobre TRI no ENEM*.
- Embretson, S.; Reise, S. *Item Response Theory for Psychologists*.
```

- [x] **Step 3: Criar `conceitos/avaliacao-diagnostica.md`**

Conteúdo:

```markdown
# Avaliação diagnóstica

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

Avaliação aplicada com objetivo de **mapear o conhecimento atual** dos alunos em determinadas habilidades, antes de iniciar um ciclo de intervenção pedagógica. Diferente de uma prova somativa, ela não tem nota final como objetivo principal — ela é insumo para a decisão pedagógica.

## Por que importa para o nosso projeto

É o ponto de partida do ciclo de trabalho do projeto. Sem o diagnóstico, não há como decidir quais descritores precisam de intervenção, como agrupar alunos, ou qual sequência didática aplicar.

## Como funciona

1. O professor escolhe os descritores que quer avaliar (vindos da BNCC / matriz curricular).
2. Aplica uma prova ou simulado cobrindo esses descritores.
3. Coleta as respostas e organiza em planilha por aluno × questão × descritor.
4. Analisa o desempenho por descritor e por aluno.

## Exemplo concreto

Início do bimestre: aplicação de uma avaliação de leitura com 15 questões cobrindo 8 descritores. Resultado é tabulado, e os alunos são agrupados por nível de leitura (ver [níveis de leitura](./niveis-de-leitura.md)).

## Limites e cuidados

- Diagnóstico que vira "prova com nota" perde valor — o aluno pode chutar sem investir nas respostas. Comunicar o propósito é parte da aplicação.
- Diagnóstico só funciona se vier intervenção depois. Diagnosticar sem agir é desperdício.

## Relação com outros conceitos

- [Descritores](./descritores.md)
- [Sequências didáticas](./sequencias-didaticas.md)
- [Níveis de leitura](./niveis-de-leitura.md)

## Referências

<!-- precisa de revisão do professor -->
```

- [x] **Step 4: Criar `conceitos/descritores.md`**

Conteúdo:

```markdown
# Descritores

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

Descritor é o nome dado a uma **habilidade curricular específica** que pode ser avaliada por uma questão. Cada descritor é codificado (por exemplo D1, D5, D11) e descreve algo como "localizar informação explícita em um texto" ou "inferir o sentido de palavra ou expressão".

A referência usada no projeto é a **Matriz de Referência SAEB**, que organiza descritores por área (Língua Portuguesa, Matemática) e por etapa escolar (5º ano, 9º ano, 3º EM).

## Por que importa para o nosso projeto

Os descritores são o eixo da análise. Em vez de dizer "o aluno foi mal em português", o projeto diz "o aluno tem dificuldade no descritor D6". Isso direciona intervenção precisa.

## Como funciona

1. Cada questão da avaliação é vinculada a um descritor (uma questão pode tocar mais de um, mas idealmente foca em um principal).
2. Após coleta, calcula-se o % de acerto **por descritor** (e não só por questão).
3. Descritores com desempenho baixo são marcados como "críticos" e priorizados na intervenção.

## Exemplo concreto

Em uma prova de 15 questões cobrindo 8 descritores, o descritor D5 (inferência) teve média de 35% de acerto na turma — sinalizando intervenção prioritária.

## Limites e cuidados

- Vinculação questão↔descritor é trabalhosa e subjetiva — pequenas diferenças podem mudar a análise.
- Nem toda matriz é a do SAEB; algumas redes adotam matrizes próprias. O projeto deve permitir configurar a matriz usada.

## Relação com outros conceitos

- [Avaliação diagnóstica](./avaliacao-diagnostica.md)
- [TCT](./teoria-classica-testes-tct.md) — TCT por descritor é uma análise muito comum.

## Referências

- INEP. *Matriz de Referência SAEB - Língua Portuguesa - 9º ano*.
- BNCC, áreas de Língua Portuguesa.
```

- [x] **Step 5: Criar `conceitos/niveis-de-leitura.md`**

Conteúdo:

```markdown
# Níveis de leitura

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

Classificação dos alunos em estágios de proficiência leitora, derivada do desempenho em avaliações de leitura. O nível guia a escolha da [sequência didática](./sequencias-didaticas.md) que cada aluno recebe.

## Por que importa para o nosso projeto

Trabalhar a turma como um bloco homogêneo é ineficaz. O agrupamento por nível permite que cada grupo receba intervenção compatível com seu ponto de partida — leitor iniciante não aprende inferência se ainda não decodifica fluentemente.

## Como funciona

<!-- precisa de revisão do professor: listar os níveis exatos usados no projeto (ex.: iniciante, em desenvolvimento, proficiente) com critérios objetivos para cada um. -->

## Exemplo concreto

<!-- precisa de revisão do professor -->

## Limites e cuidados

- O nível é uma fotografia, não uma sentença. Alunos transitam entre níveis ao longo do ano.
- Comunicar o nível ao aluno exige cuidado pedagógico — evitar rótulos estigmatizantes.

## Relação com outros conceitos

- [Sequências didáticas](./sequencias-didaticas.md)
- [Avaliação diagnóstica](./avaliacao-diagnostica.md)

## Referências

<!-- precisa de revisão do professor -->
```

- [x] **Step 6: Criar `conceitos/sequencias-didaticas.md`**

Conteúdo:

```markdown
# Sequências didáticas

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

Conjunto encadeado de atividades planejadas para desenvolver um conjunto específico de habilidades (descritores) num determinado período. Cada sequência tem entrada (o que o aluno precisa saber para começar), atividades e produto final.

## Por que importa para o nosso projeto

A sequência didática é a **resposta** ao diagnóstico. Diagnóstico sem sequência fica em "dado bonito sem ação". O projeto pretende, no futuro, sugerir sequências automaticamente a partir dos descritores críticos.

## Como funciona

1. A partir dos descritores críticos identificados na avaliação, o professor seleciona ou cria uma sequência didática.
2. A sequência é aplicada com o grupo correspondente.
3. Ao final, uma reavaliação mede o efeito.

A curadoria de **gêneros textuais** (notícia, conto, verbete, artigo de opinião…) é parte da escolha — o gênero deve ser adequado tanto ao descritor quanto ao nível.

## Exemplo concreto

<!-- precisa de revisão do professor -->

## Limites e cuidados

- Sequência curta demais não consolida; longa demais perde foco.
- Reavaliação imediata mede memória recente, não aprendizagem consolidada — espaçar a reavaliação ajuda.

## Relação com outros conceitos

- [Descritores](./descritores.md)
- [Níveis de leitura](./niveis-de-leitura.md)

## Referências

- Dolz, J.; Noverraz, M.; Schneuwly, B. *Sequências didáticas para o oral e a escrita*.
```

- [x] **Step 7: Criar `conceitos/escala-saepe.md`**

Conteúdo:

```markdown
# Escala SAEPE

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

Escala numérica de **0 a 500** usada para reportar proficiência em avaliações em larga escala no Sistema de Avaliação Educacional de Pernambuco (SAEPE). É a mesma família de escala usada em SAEB e Prova Brasil.

## Por que importa para o nosso projeto

A saída final do cálculo de desempenho do aluno (após TCT + TRI simplificada) é convertida para essa escala. Isso permite comparar o desempenho interno com referências oficiais conhecidas pelos professores e gestores.

## Como funciona

Após calcular a nota real ponderada (`AX` na planilha) e convertê-la em percentual (`AY`), aplica-se uma transformação linear que leva o resultado para a faixa 0-500. A fórmula exata e a calibração estão em [`../formulas/tri-simplificado.md`](../formulas/tri-simplificado.md).

## Faixas de proficiência (referência)

<!-- precisa de revisão do professor para indicar as faixas oficiais SAEPE por etapa (ex.: até X = abaixo do básico; X-Y = básico; etc.) -->

## Limites e cuidados

- Escala SAEPE oficial é calibrada com TRI completa; nossa conversão é aproximada via TRI simplificada — diferenças são esperadas e devem ser comunicadas em relatórios.

## Relação com outros conceitos

- [TRI simplificada](./teoria-resposta-item-tri.md)
- [TCT](./teoria-classica-testes-tct.md)

## Referências

- Secretaria de Educação de Pernambuco — documentos SAEPE.
```

- [x] **Step 8: Commit**

```bash
git add docs/dominio/conceitos/
git commit -m "docs(dominio): semear conceitos a partir do post (TCT, TRI, descritores, niveis, sequencias, SAEPE)"
```

---

## Task 5: Popular `dominio/processos/`

**Files:**
- Create: `docs/dominio/processos/ciclo-avaliativo.md`
- Create: `docs/dominio/processos/coleta-de-dados.md`
- Create: `docs/dominio/processos/analise-de-desempenho.md`
- Create: `docs/dominio/processos/agrupamento-de-alunos.md`
- Create: `docs/dominio/processos/relatorios-pedagogicos.md`

- [x] **Step 1: Criar `processos/ciclo-avaliativo.md`**

```markdown
# Processo: Ciclo Avaliativo

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## Objetivo do processo

Aplicar avaliações diagnósticas e simulados de forma sistemática, gerando insumo para intervenções pedagógicas direcionadas.

## Quando ele acontece

Tipicamente uma ou mais vezes por bimestre, alinhado ao calendário escolar do 8º e 9º anos.

## Pré-requisitos

- Matriz de descritores definida.
- Instrumento de avaliação (prova ou simulado) construído ou selecionado.
- Turmas alocadas e horário reservado.

## Passo a passo (como é feito hoje)

1. Seleção de descritores a avaliar.
2. Construção/curadoria da prova cobrindo esses descritores.
3. Aplicação da prova em sala.
4. Coleta das respostas (cartão-resposta ou prova física).
5. Tabulação em planilha (ver [`coleta-de-dados.md`](./coleta-de-dados.md)).
6. Análise (ver [`analise-de-desempenho.md`](./analise-de-desempenho.md)).
7. Agrupamento de alunos por perfil (ver [`agrupamento-de-alunos.md`](./agrupamento-de-alunos.md)).
8. Aplicação da sequência didática de intervenção.
9. Reavaliação ao final do ciclo.

## Artefatos produzidos

- Planilha consolidada por turma.
- Relatório por descritor.
- Relatório por aluno.
- Lista de descritores críticos.
- Agrupamentos com sequência didática associada.

## Pontos de dor

- Tabulação manual demora.
- Análise por descritor é trabalhosa em planilha.
- Geração de relatório por aluno hoje é manual com apoio de IA.
- Falta padronização entre simulados.

## O que seria ideal

Sistema que receba os dados (CSV/planilha) e gere automaticamente: análise por descritor, % de acerto por questão, agrupamento por nível, identificação de descritores críticos e relatórios pedagógicos prontos por aluno e por turma.

## Conceitos relacionados

- [Avaliação diagnóstica](../conceitos/avaliacao-diagnostica.md)
- [Descritores](../conceitos/descritores.md)
- [Sequências didáticas](../conceitos/sequencias-didaticas.md)
```

- [x] **Step 2: Criar `processos/coleta-de-dados.md`**

```markdown
# Processo: Coleta de Dados

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## Objetivo do processo

Transformar as respostas em papel em uma planilha estruturada que possa ser analisada.

## Quando ele acontece

Logo após a aplicação da avaliação, antes da análise.

## Pré-requisitos

- Prova corrigida (gabarito disponível).
- Cartões-resposta ou provas físicas dos alunos.
- Planilha modelo preparada.

## Passo a passo (como é feito hoje)

1. Para cada aluno, marca-se acerto/erro por questão na planilha.
2. Atribui-se cor à questão conforme classificação TCT (verde = fácil, amarelo = médio, vermelho = difícil). O Claude ajudou a construir um código que faz essa contagem automática por cores.
3. Calcula-se totalizadores por aluno e por questão.

## Artefatos produzidos

- Planilha por turma com colunas mapeadas:
  - `AN/AO` — erros em fáceis e %
  - `AP/AQ` — erros em médias e %
  - `AR/AS` — erros em difíceis e %
  - `AT/AU/AV` — pontos corrigidos F/M/D
  - `AW` — ajuste de pontos das difíceis
  - `AX` — nota real ponderada
  - `AY` — nota em %
  - `AZ` — nota convertida em escala SAEPE (0-500)

## Pontos de dor

- Trabalho manual significativo.
- Dependência de cores na planilha é frágil (mudar visual quebra o cálculo).
- Sem validação automática de inconsistências.

## O que seria ideal

Importação direta de CSV/Excel com os acertos por aluno × questão. O sistema deve aplicar TCT/TRI automaticamente, sem depender de cores.

## Conceitos relacionados

- [TCT](../conceitos/teoria-classica-testes-tct.md)
- [TRI simplificada](../conceitos/teoria-resposta-item-tri.md)
- [Fórmula TRI simplificada](../formulas/tri-simplificado.md)
```

- [x] **Step 3: Criar `processos/analise-de-desempenho.md`**

```markdown
# Processo: Análise de Desempenho

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## Objetivo do processo

Identificar padrões de erro, descritores críticos e níveis de aluno a partir dos dados coletados.

## Quando ele acontece

Após a [coleta de dados](./coleta-de-dados.md).

## Pré-requisitos

- Planilha consolidada por turma.

## Passo a passo (como é feito hoje)

1. Calcular % de acerto por questão.
2. Classificar questões em fácil/médio/difícil via [TCT](../conceitos/teoria-classica-testes-tct.md).
3. Calcular % de acerto por descritor.
4. Aplicar [TRI simplificada](../conceitos/teoria-resposta-item-tri.md) para gerar nota ponderada.
5. Converter para escala SAEPE (`AZ`).
6. Marcar descritores com baixo desempenho como críticos.
7. Identificar perfil de cada aluno (qual nível de leitura, quais descritores são pontos fortes/fracos).

## Artefatos produzidos

- Lista de descritores críticos.
- Tabela aluno × perfil.
- Comparativo por turma.

## Pontos de dor

- Cruzamento manual de aluno × descritor.
- Geração de visualizações claras é trabalhosa.

## O que seria ideal

Dashboard com filtros por descritor, turma, aluno, nível. Identificação automática de descritores críticos por limiar configurável.

## Conceitos relacionados

- [Descritores](../conceitos/descritores.md)
- [Níveis de leitura](../conceitos/niveis-de-leitura.md)
```

- [x] **Step 4: Criar `processos/agrupamento-de-alunos.md`**

```markdown
# Processo: Agrupamento de Alunos

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## Objetivo do processo

Organizar alunos em grupos por perfil de desempenho, para que cada grupo receba intervenção adequada.

## Quando ele acontece

Após [análise de desempenho](./analise-de-desempenho.md) e antes da aplicação das sequências didáticas.

## Pré-requisitos

- Perfis individuais identificados na análise.

## Passo a passo (como é feito hoje)

1. Listar alunos com seus perfis (nível de leitura, descritores fracos).
2. Agrupar por similaridade de perfil.
3. Dimensionar grupos com base no espaço/tempo disponível.
4. Alocar a sequência didática mais adequada a cada grupo.

## Artefatos produzidos

- Lista de grupos com nomes de alunos.
- Sequência didática associada a cada grupo.

## Pontos de dor

- Agrupar manualmente quando há muitos alunos com perfis diferentes é trabalhoso.
- Equilibrar grupos (não deixar um grupo gigantesco) exige iteração.

## O que seria ideal

Sugestão automática de agrupamento com restrições configuráveis (tamanho máximo do grupo, balanceamento por descritor).

## Conceitos relacionados

- [Níveis de leitura](../conceitos/niveis-de-leitura.md)
- [Sequências didáticas](../conceitos/sequencias-didaticas.md)
```

- [x] **Step 5: Criar `processos/relatorios-pedagogicos.md`**

```markdown
# Processo: Relatórios Pedagógicos

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## Objetivo do processo

Comunicar os resultados das avaliações em formato útil para o professor, o aluno, a família e a gestão escolar.

## Quando ele acontece

Após a análise, antes ou durante a aplicação das intervenções (para que sirva como base de conversa pedagógica).

## Pré-requisitos

- Análise concluída.

## Passo a passo (como é feito hoje)

1. Para cada aluno, gerar um texto resumindo perfil, pontos fortes e descritores a trabalhar.
2. Para cada turma, gerar um relatório com descritores críticos e proposta de intervenção.
3. Hoje, o professor faz isso com apoio de IA (cola dados na conversa, recebe rascunho de relatório).

## Artefatos produzidos

- Relatório individual por aluno.
- Relatório consolidado por turma.

## Pontos de dor

- Tempo gasto na geração manual mesmo com apoio de IA.
- Formato não padronizado entre avaliações.
- Dificuldade em manter histórico ao longo do ano.

## O que seria ideal

Sistema que gere o relatório individual e por turma a partir dos dados, com template configurável e histórico longitudinal (evolução do aluno ao longo do ano).

## Conceitos relacionados

- [Avaliação diagnóstica](../conceitos/avaliacao-diagnostica.md)
- [Descritores](../conceitos/descritores.md)
```

- [x] **Step 6: Commit**

```bash
git add docs/dominio/processos/
git commit -m "docs(dominio): documentar processos (ciclo, coleta, analise, agrupamento, relatorios)"
```

---

## Task 6: Criar `dominio/formulas/tri-simplificado.md`

**Files:**
- Create: `docs/dominio/formulas/tri-simplificado.md`

- [x] **Step 1: Criar arquivo com fórmula completa do post**

⚠️ STABILITY-CRITICAL — pausar e confirmar com humano antes de executar este step (matemática do projeto).

Conteúdo:

````markdown
# Fórmula: TRI Simplificada (TCT + Penalização + Ponderação)

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli
>
> ⚠️ Área sensível: revisão pedagógica obrigatória antes de mudar qualquer fórmula desta página.

## Para que serve

Calcular a nota de cada aluno de forma que reflita não só a quantidade de acertos, mas também a dificuldade das questões e a probabilidade de chute. Saída final é convertida para a escala SAEPE (0 a 500).

## Visão geral do cálculo

```
Acertos brutos (por dificuldade)
        ↓
Penalização do acerto ao acaso (descontar chute)
        ↓
Ajuste específico das questões fáceis (caso especial)
        ↓
Nota real ponderada (peso por dificuldade)
        ↓
Conversão para escala SAEPE (0-500)
```

## Variáveis (mapeamento de colunas da planilha)

| Coluna | Significado                                                             |
|--------|-------------------------------------------------------------------------|
| `AN`   | Quantidade de erros em questões **fáceis**                              |
| `AO`   | % de erros em questões fáceis                                           |
| `AP`   | Quantidade de erros em questões **médias**                              |
| `AQ`   | % de erros em questões médias                                           |
| `AR`   | Quantidade de erros em questões **difíceis**                            |
| `AS`   | % de erros em questões difíceis                                         |
| `AT`   | Pontos corrigidos das fáceis (após penalização de chute)                |
| `AU`   | Pontos corrigidos das médias                                            |
| `AV`   | Pontos corrigidos das difíceis (antes do ajuste fácil)                  |
| `AW`   | Ajuste dos pontos das difíceis (passada por SE condicional)             |
| `AX`   | Nota real ponderada por dificuldade                                     |
| `AY`   | Nota real convertida em %                                               |
| `AZ`   | Nota transformada na escala SAEPE (0 a 500)                             |
| `AN$1`, `AP$1`, `AR$1` | Totais máximos de questões fáceis/médias/difíceis (cabeçalho) |

## Fórmulas

### Penalização do acerto ao acaso (por categoria)

A intuição é descontar uma fração estimada de chute proporcional ao número de erros — quem errou muito provavelmente também chutou em algumas das acertadas.

```excel
AT3 = ($AN$1 - AN3) - (AN3 / 3)   # fáceis
AU3 = ($AP$1 - AP3) - (AP3 / 3)   # médias
AV3 = ($AR$1 - AR3) - (AR3 / 3)   # difíceis
```

Leitura: "(total de fáceis menos erros nas fáceis) menos um terço dos erros nas fáceis".

### Ajuste especial das fáceis (`AW`)

Para questões com mais de 50% de erro entre as fáceis, aplica-se uma compensação:

```excel
AW3 = SE((13 - AN3) / 13 < 0,5; MÍNIMO(AV3; 0) + MÁXIMO(AV3; 0) * 0,5; AV3)
```

Leitura: "se o aluno acertou menos de 50% das fáceis, suaviza o `AV`; caso contrário, mantém `AV`".

> O `13` aqui é o total de questões fáceis na prova de referência. Esse valor deve ser parametrizado quando virar código — ele mudará a cada prova.

### Nota real ponderada (`AX`)

```excel
AX3 = MÁXIMO(AT3; 0) * 1 + MÁXIMO(AU3; 0) * 1,5 + AW3 * 2
```

Pesos:
- Fácil = 1
- Média = 1,5
- Difícil = 2

### Conversão para escala SAEPE (`AZ`)

<!-- precisa de revisão do professor para incluir a fórmula exata de transformação linear de AY% para a faixa 0-500. -->

## Exemplo numérico

<!-- precisa de revisão do professor: caso concreto de um aluno com valores reais nas colunas AN..AZ. -->

## Por que essa fórmula foi escolhida

Versão simplificada da TRI, calibrada empiricamente, que captura as duas intuições centrais (penalizar chute, ponderar dificuldade) sem precisar de estimação estatística de parâmetros de item — inviável de fazer manualmente em planilha.

## Limites conhecidos

- Fator `/3` na penalização é heurística — assume probabilidade média de chute de 1/3 (típico de prova de múltipla escolha com 4 alternativas viáveis).
- Peso 1 / 1,5 / 2 é calibração empírica; outra escolha poderia produzir resultados melhores em outros contextos.
- O `13` hardcoded é frágil — quando virar sistema, deve ser parâmetro.

## Referências

- Post original do prof. Alikaeli (arquivo `post_alikaeli` na raiz do repo).
- [TCT](../conceitos/teoria-classica-testes-tct.md), [TRI simplificada](../conceitos/teoria-resposta-item-tri.md), [Escala SAEPE](../conceitos/escala-saepe.md).
````

- [x] **Step 2: Commit**

```bash
git add docs/dominio/formulas/tri-simplificado.md
git commit -m "docs(dominio): documentar formula TRI simplificada com mapeamento de colunas"
```

---

## Task 7: Criar convenções e MAP em `docs/llm/`

**Files:**
- Create: `docs/llm/conventions/writing-slices.md`
- Create: `docs/llm/conventions/context-engineering.md`
- Create: `docs/llm/MAP.md`
- Create: `docs/llm/GLOSSARY.md`

- [x] **Step 1: Criar `docs/llm/conventions/writing-slices.md`**

```markdown
# Como escrever slices

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Slices em `docs/llm/slices/` são pedaços compactos de documentação carregados sob demanda pelo agente LLM em `/research` e `/plan`. Otimizados para **densidade**, não para leitura humana confortável.

## Regras de ouro

- **≤200 linhas por slice.** Se passa disso, divida.
- **Frontmatter YAML obrigatório** com:
  - `id` — identificador único em kebab-case.
  - `title` — título legível.
  - `when-to-load` — lista de triggers (palavras/expressões) que indicam que esse slice é relevante.
  - `owners-paths` — globs dos arquivos que esse slice cobre (em `docs/dominio/` ou `src/`).
  - `last-verified` — data ISO do último cross-check com a fonte.
  - `version` — inteiro, incrementa em mudança substantiva.
- **Anchors estáveis** — use cabeçalhos `## <Nome>` que não mudam à toa. Outros docs podem referenciar `slices/foo.md#nome-anchor`.
- **Cite a fonte** — toda afirmação importante referencia um arquivo de domínio ou linha de código.
- **Tom telegráfico.** Bullet points, tabelas, frases curtas. Prose explicativa vai em `docs/dominio/` ou `docs/dev/`.

## Esqueleto

```markdown
---
id: meu-slice
title: Meu Slice
when-to-load: ["palavra1", "expressao 2", "termo 3"]
owners-paths: ["docs/dominio/conceitos/meu-conceito.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

<2-3 bullets capturando o essencial>

## Fatos-chave

- <fato 1, com referência>
- <fato 2>

## Cuidados

- <armadilha conhecida>

## Para aprofundar

- [`docs/dominio/.../arquivo.md`](../../dominio/...)
```

## O que NÃO colocar em slice

- Onboarding humano (vai em `docs/dominio/README.md` ou `docs/dev/README.md`).
- Exemplos longos e ilustrativos (vão em `docs/dominio/`).
- Tutoriais (vão em `docs/dev/`).

## Quando criar slice novo

- Surgiu uma área que `/research` precisa carregar e que ainda não tem slice.
- Slice existente passou de 200 linhas → divida em dois.
- Novo conceito grande chegou em `docs/dominio/` → crie slice espelho.
```

- [x] **Step 2: Criar `docs/llm/conventions/context-engineering.md`**

```markdown
# Engenharia de contexto

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Princípios para agentes LLM neste repo carregarem **só o necessário, na hora certa**.

## Princípios

1. **Sempre carregue `MAP.md` + `GLOSSARY.md`.** São o índice e o vocabulário comum.
2. **Carregue slices sob demanda** — máximo 3 por research; mais que isso, divida em sub-research.
3. **Cite arquivos com paths exatos.** "Procure no domínio" não basta — diga "docs/dominio/conceitos/tct.md".
4. **Trust the code over the docs.** Se um slice contradiz a fonte, sinalize e proponha `/verify-and-doc`.
5. **PT-BR para termos de domínio.** EN para conceitos técnicos de implementação (greenfield ainda, mas vale para o futuro).

## Como `/research` decide o que carregar

1. Lê `MAP.md` e identifica triggers no tópico do usuário.
2. Lê `GLOSSARY.md` (sempre).
3. Para cada slice com trigger match, lê o slice inteiro.
4. Se nenhum slice der match, opera com MAP + GLOSSARY apenas e sinaliza no output.

## Como `/plan` consome o output de `/research`

1. Lê os slices que `/research` listou em "slices to load if implementing".
2. Lê `conventions/context-engineering.md` (este arquivo) e `conventions/writing-slices.md` se for criar slice novo.
3. Gera plano com paths exatos.
4. Marca tasks stability-critical (fórmulas, definições teóricas, mapeamento de descritores).

## Como `/verify-and-doc` mantém slices vivos

1. Roda diff contra o último commit.
2. Para cada arquivo alterado em `docs/dominio/` ou `src/`, busca slices com `owners-paths` casando.
3. Atualiza slice afetado, refresca `last-verified`, bumpa `version` se houve mudança substantiva.
4. Cria slice novo se diff trouxe área não-coberta.
```

- [x] **Step 3: Criar `docs/llm/MAP.md`**

```markdown
# MAP (LLM Index)

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Índice global. Sempre carregado pelo agente em `/research` e `/plan`. Lista slices disponíveis com triggers.

## Layout do repositório

```
/
├── post_alikaeli              # post original do professor (contexto humano)
├── README.md                  # vitrine + ponteiros
├── docs/
│   ├── INDEX.md               # mapa das pastas
│   ├── dominio/               # FONTE CANÔNICA de domínio (PT-BR)
│   ├── dev/                   # docs para devs
│   ├── llm/                   # este diretório (slices + convenções)
│   └── superpowers/
│       ├── specs/             # designs aprovados
│       └── plans/             # planos executáveis
└── .claude/
    ├── agents/                # research, plan, implement, verify-and-document
    └── commands/              # /research, /plan, /implement, /verify-and-doc
```

## Quando carregar qual slice

| Se o tópico do usuário contém…                         | Carregue                          |
|--------------------------------------------------------|-----------------------------------|
| "avaliação", "diagnóstica", "simulado", "ciclo"        | `slices/dominio-avaliacao-ciclo.md` |
| "descritor", "matriz SAEB", "BNCC", "habilidade"       | `slices/dominio-descritores.md`     |
| "TCT", "teoria clássica", "fácil/médio/difícil"        | `slices/dominio-tct.md`             |
| "TRI", "penalização", "chute", "ponderação", "SAEPE"   | `slices/dominio-tri-simplificado.md` |
| "análise", "agrupamento", "nível", "intervenção"       | `slices/dominio-analise-pedagogica.md` |
| "relatório", "feedback", "comunicação de resultado"    | `slices/dominio-relatorios.md`      |

## Disambiguation traps

- **Dois glossários:** `docs/dominio/GLOSSARIO.md` (completo, humano) vs. `docs/llm/GLOSSARY.md` (compacto, agente). Fonte da verdade é o de domínio.
- **TRI no projeto ≠ TRI oficial:** o que chamamos de TRI é a versão simplificada. Para TRI oficial do ENEM/SAEB, deixar claro.
- **Pré-código:** nenhum arquivo em `src/` ainda. `/research` em tópicos de implementação cai apenas em domínio.

## Convenções

- Veja [`conventions/writing-slices.md`](./conventions/writing-slices.md) e [`conventions/context-engineering.md`](./conventions/context-engineering.md).
```

- [x] **Step 4: Criar `docs/llm/GLOSSARY.md` (versão compacta)**

```markdown
# GLOSSARY (LLM compact)

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu
>
> Versão compacta. Para definições completas com exemplos, ver [`docs/dominio/GLOSSARIO.md`](../dominio/GLOSSARIO.md).

| Termo | Definição (uma linha) |
|-------|------------------------|
| Avaliação diagnóstica | Avaliação para mapear conhecimento atual antes de intervenção. |
| Agrupamento por nível | Organizar alunos por perfil para receber intervenção específica. |
| Descritor | Habilidade curricular avaliada por uma questão; codificada (D1, D5…). Referência: Matriz SAEB. |
| Escala SAEPE | Escala 0-500 de proficiência usada como saída final. |
| Gênero textual | Tipo de texto (notícia, conto…) usado em sequência didática. |
| Intervenção pedagógica | Atividades planejadas para superar dificuldade diagnosticada. |
| Nível de leitura | Classificação do aluno em estágios de proficiência leitora. |
| Penalização de acerto ao acaso | Desconto no escore para reduzir contribuição de chute. |
| Sequência didática | Conjunto encadeado de atividades para desenvolver habilidades. |
| Simulado | Avaliação simulando provas externas (ENEM/SAEB/SAEPE). |
| TCT | Teoria Clássica dos Testes — classifica questão por % de acerto (fácil >70%, médio 50-70%, difícil ≤50%). |
| TRI | Teoria de Resposta ao Item. No projeto, versão simplificada: TCT + penalização + ponderação. |
```

- [x] **Step 5: Commit**

```bash
git add docs/llm/conventions/ docs/llm/MAP.md docs/llm/GLOSSARY.md
git commit -m "docs(llm): adicionar MAP, GLOSSARY e convencoes de slice"
```

---

## Task 8: Criar slices iniciais em `docs/llm/slices/`

**Files:**
- Create: `docs/llm/slices/dominio-avaliacao-ciclo.md`
- Create: `docs/llm/slices/dominio-descritores.md`
- Create: `docs/llm/slices/dominio-tct.md`
- Create: `docs/llm/slices/dominio-tri-simplificado.md`
- Create: `docs/llm/slices/dominio-analise-pedagogica.md`
- Create: `docs/llm/slices/dominio-relatorios.md`

- [x] **Step 1: `slices/dominio-tct.md`**

```markdown
---
id: dominio-tct
title: TCT (Teoria Clássica dos Testes)
when-to-load: ["TCT", "teoria clássica", "fácil médio difícil", "classificação de questão"]
owners-paths: ["docs/dominio/conceitos/teoria-classica-testes-tct.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- TCT classifica questões por proporção de acertos da turma.
- Saída: faixa fácil/médio/difícil que alimenta cálculo de nota ponderada.
- Entrada da [TRI simplificada](./dominio-tri-simplificado.md).

## Faixas (regra do projeto)

| Acertos da turma          | Classificação |
|---------------------------|---------------|
| > 70%                     | Fácil         |
| > 50% e ≤ 70%             | Médio         |
| ≤ 50% (erro > 50%)        | Difícil       |

## Fatos-chave

- Classificação depende da turma — não é absoluta.
- TCT por **descritor** é análise comum: % de acerto agrupada por habilidade.
- TCT é entrada da nossa TRI simplificada (não substitui).

## Cuidados

- Não comparar diretamente fácil/médio/difícil entre turmas — a mesma questão muda de classe.
- Não confundir com TRI oficial (ENEM/SAEB usam modelo estatístico estimado, não proporção simples).

## Para aprofundar

- [`docs/dominio/conceitos/teoria-classica-testes-tct.md`](../../dominio/conceitos/teoria-classica-testes-tct.md)
- [`docs/dominio/formulas/tri-simplificado.md`](../../dominio/formulas/tri-simplificado.md)
```

- [x] **Step 2: `slices/dominio-tri-simplificado.md`**

```markdown
---
id: dominio-tri-simplificado
title: TRI Simplificada do Projeto
when-to-load: ["TRI", "penalização", "chute", "ponderação", "nota ponderada", "SAEPE", "fórmula"]
owners-paths: ["docs/dominio/conceitos/teoria-resposta-item-tri.md", "docs/dominio/formulas/tri-simplificado.md", "docs/dominio/conceitos/escala-saepe.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- Versão simplificada da TRI, não a oficial do ENEM.
- Combina TCT + penalização de chute + ponderação por dificuldade.
- Saída final convertida para escala SAEPE (0-500).

## Pipeline

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

## Fatos-chave

- Pesos: fácil = 1, médio = 1,5, difícil = 2.
- Penalização assume probabilidade média de chute ~1/3 (4 alternativas).
- Hardcoded `13` (total de fáceis na prova de referência) → vira parâmetro quando virar sistema.

## Cuidados

- ⚠️ Stability-critical: mudar fórmula altera análise de centenas de alunos.
- Pesos foram calibrados empiricamente — não trocar sem revisão pedagógica.
- Saída SAEPE é aproximação; comunicar isso em relatórios.

## Para aprofundar

- [`docs/dominio/formulas/tri-simplificado.md`](../../dominio/formulas/tri-simplificado.md) — fórmulas completas com mapeamento de colunas.
- [`docs/dominio/conceitos/teoria-resposta-item-tri.md`](../../dominio/conceitos/teoria-resposta-item-tri.md) — teoria.
- [`docs/dominio/conceitos/escala-saepe.md`](../../dominio/conceitos/escala-saepe.md) — saída final.
```

- [x] **Step 3: `slices/dominio-descritores.md`**

```markdown
---
id: dominio-descritores
title: Descritores
when-to-load: ["descritor", "matriz SAEB", "BNCC", "habilidade", "D1", "D5"]
owners-paths: ["docs/dominio/conceitos/descritores.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- Descritor = habilidade curricular avaliada por uma questão.
- Codificação (D1, D5…) vem da Matriz SAEB (referência principal do projeto).
- Análise por descritor é o eixo da intervenção pedagógica do projeto.

## Fatos-chave

- Uma questão pode tocar múltiplos descritores; tipicamente é vinculada a um principal.
- Descritor com baixo desempenho = **descritor crítico** = prioridade na intervenção.
- Matriz pode variar por rede de ensino — sistema deve permitir matriz configurável.

## Cuidados

- Vinculação questão↔descritor é trabalhosa e subjetiva.
- BNCC ≠ Matriz SAEB; nem todo descritor SAEB casa 1:1 com competência BNCC.

## Para aprofundar

- [`docs/dominio/conceitos/descritores.md`](../../dominio/conceitos/descritores.md)
- INEP: Matriz de Referência SAEB - Língua Portuguesa - 9º ano (link oficial em referências do conceito).
```

- [x] **Step 4: `slices/dominio-avaliacao-ciclo.md`**

```markdown
---
id: dominio-avaliacao-ciclo
title: Ciclo Avaliativo
when-to-load: ["avaliação", "diagnóstica", "simulado", "ciclo", "aplicação"]
owners-paths: ["docs/dominio/conceitos/avaliacao-diagnostica.md", "docs/dominio/processos/ciclo-avaliativo.md", "docs/dominio/processos/coleta-de-dados.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- Ciclo: seleção de descritores → aplicação → coleta → análise → agrupamento → intervenção → reavaliação.
- Público: 8º e 9º anos.
- Diagnóstico sem intervenção subsequente = desperdício.

## Fluxo

```
1. Selecionar descritores                  [conceitos/descritores]
2. Construir prova                          [processos/ciclo-avaliativo]
3. Aplicar em sala
4. Tabular respostas em planilha            [processos/coleta-de-dados]
5. Analisar por descritor + classificar TCT [conceitos/tct]
6. Aplicar TRI simplificada                 [formulas/tri-simplificado]
7. Agrupar alunos por nível                 [processos/agrupamento]
8. Aplicar sequência didática               [conceitos/sequencias-didaticas]
9. Reavaliar
```

## Fatos-chave

- Hoje tudo é manual com apoio de IA + planilha.
- Coleta usa cores na planilha (frágil) — sistema deve substituir por importação CSV/Excel.
- Reavaliação ao final do ciclo é o sinal de eficácia.

## Cuidados

- "Avaliação diagnóstica com nota" vira prova somativa e perde valor diagnóstico.
- Coleta dependente de cores quebra se o visual da planilha mudar.

## Para aprofundar

- [`docs/dominio/processos/ciclo-avaliativo.md`](../../dominio/processos/ciclo-avaliativo.md)
- [`docs/dominio/processos/coleta-de-dados.md`](../../dominio/processos/coleta-de-dados.md)
```

- [x] **Step 5: `slices/dominio-analise-pedagogica.md`**

```markdown
---
id: dominio-analise-pedagogica
title: Análise Pedagógica e Agrupamento
when-to-load: ["análise", "agrupamento", "nível", "intervenção", "perfil"]
owners-paths: ["docs/dominio/processos/analise-de-desempenho.md", "docs/dominio/processos/agrupamento-de-alunos.md", "docs/dominio/conceitos/niveis-de-leitura.md", "docs/dominio/conceitos/sequencias-didaticas.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- Análise transforma planilha bruta em insights acionáveis: descritores críticos, perfis de aluno, agrupamentos.
- Agrupamento por nível guia a escolha da sequência didática.

## Pipeline de análise

```
Dados coletados
   ↓
% de acerto por questão (TCT)
% de acerto por descritor
   ↓
Marcar descritores críticos (limiar configurável)
Identificar perfil de cada aluno
   ↓
Agrupar alunos por perfil
   ↓
Associar sequência didática a cada grupo
```

## Fatos-chave

- Hoje feito em planilha + apoio de IA. Trabalhoso.
- Limiar de "crítico" não é fixo — depende do contexto da turma.
- Agrupamento equilibrado (tamanhos parecidos) exige iteração manual.

## Cuidados

- Não rotular o aluno permanentemente pelo nível — comunicação importa.
- Reavaliar agrupamentos a cada ciclo (alunos transitam).

## Para aprofundar

- [`docs/dominio/processos/analise-de-desempenho.md`](../../dominio/processos/analise-de-desempenho.md)
- [`docs/dominio/processos/agrupamento-de-alunos.md`](../../dominio/processos/agrupamento-de-alunos.md)
- [`docs/dominio/conceitos/niveis-de-leitura.md`](../../dominio/conceitos/niveis-de-leitura.md)
```

- [x] **Step 6: `slices/dominio-relatorios.md`**

```markdown
---
id: dominio-relatorios
title: Relatórios Pedagógicos
when-to-load: ["relatório", "feedback", "comunicação", "devolutiva"]
owners-paths: ["docs/dominio/processos/relatorios-pedagogicos.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- Dois tipos de relatório: individual (aluno) e consolidado (turma).
- Hoje gerados manualmente com apoio de IA — gargalo principal do trabalho do professor.
- Histórico longitudinal (evolução ao longo do ano) não existe hoje.

## Fatos-chave

- Cada relatório individual resume: perfil, pontos fortes, descritores a trabalhar.
- Relatório de turma lista: descritores críticos, proposta de intervenção, agrupamentos.

## Cuidados

- Tom precisa ser pedagógico, não punitivo. Rotular o aluno como "fraco" é contraproducente.
- Padronização de formato facilita comparação ano a ano.

## Para aprofundar

- [`docs/dominio/processos/relatorios-pedagogicos.md`](../../dominio/processos/relatorios-pedagogicos.md)
```

- [x] **Step 7: Commit**

```bash
git add docs/llm/slices/
git commit -m "docs(llm): adicionar slices iniciais espelhando dominio"
```

---

## Task 9: Criar `docs/dev/` (3 arquivos)

**Files:**
- Create: `docs/dev/README.md`
- Create: `docs/dev/AI-WORKFLOW.md`
- Create: `docs/dev/COMO-CONTRIBUIR.md`

- [x] **Step 1: `docs/dev/README.md`**

```markdown
# Documentação Dev

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Documentação técnica voltada para devs humanos. Cresce conforme o projeto evolui.

## O que vive aqui hoje

- [`AI-WORKFLOW.md`](./AI-WORKFLOW.md) — tutorial do workflow Research → Plan → Implement → Verify usado neste repo.
- [`COMO-CONTRIBUIR.md`](./COMO-CONTRIBUIR.md) — como contribuir (devs e não-devs, incluindo o professor).

## O que virá aqui no futuro

Quando começarmos a implementar:

- `ARQUITETURA.md` — visão técnica do sistema.
- `STACK.md` — escolhas de tecnologia e por quê.
- `SETUP.md` — como rodar localmente.
- `TROUBLESHOOTING.md` — problemas conhecidos.

> Princípio: não criar placeholder vazio. O doc nasce quando há conteúdo real para ele.

## Princípios

- Cada doc top-level tem `> Última atualização: AAAA-MM-DD · Mantenedor: <quem>`.
- Cita arquivos com path exato (`docs/dominio/conceitos/foo.md`, ou `src/foo.js:123` quando houver código).
- Não duplica `docs/dominio/` — referencia.

## Para entender o repo todo

Comece em [`../INDEX.md`](../INDEX.md).
```

- [x] **Step 2: `docs/dev/AI-WORKFLOW.md`**

```markdown
# Workflow com IA / LLMs (R-P-I-V)

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Tutorial prático do workflow **Research → Plan → Implement → Verify** usado neste repo. Documento vivo — atualize conforme evoluímos.

## Por que esse workflow

Repo pequeno hoje, mas o domínio é técnico (pedagogia + psicometria) e o time inclui um professor (não-dev). Pedir para o LLM "implementa X" sem contexto leva a:

- Decisões erradas (LLM inventa solução que ignora o conteúdo pedagógico real).
- Mexer em fórmulas sem revisão (área sensível).
- Tempo perdido em "ah, mas aqui é diferente".

Solução: documentação **LLM-first** em `docs/llm/` (slices compactos) + comandos slash que materializam o fluxo R-P-I-V.

## Os 4 pilares

### 1. `docs/llm/MAP.md`

Índice global. Lista todas as slices com triggers ("se o tópico é X, carregue slice Y"). **Sempre** carregado pelo agente de research.

### 2. `docs/llm/GLOSSARY.md`

Vocabulário PT-BR compacto. Também sempre carregado. Versão completa em `docs/dominio/GLOSSARIO.md`.

### 3. `docs/llm/slices/*.md`

Slices carregadas **sob demanda**, cada uma com frontmatter (`when-to-load`, `owners-paths`, `last-verified`). ≤200 linhas, tom telegráfico.

Slices atuais (espelham `docs/dominio/`):
- `dominio-avaliacao-ciclo`, `dominio-descritores`, `dominio-tct`, `dominio-tri-simplificado`, `dominio-analise-pedagogica`, `dominio-relatorios`.

### 4. `.claude/` (agents + commands)

Os 4 subagentes (`research`, `plan`, `implement`, `verify-and-document`) e os 4 slash commands correspondentes. Adaptados para o contexto educacional deste repo.

## R-P-I-V em 5 minutos

### `/research <tópico>`

Investigação read-only. Carrega MAP + GLOSSARY + slices relevantes. Retorna síntese estruturada (arquivos de interesse, mental model, perguntas em aberto, slices a carregar se for implementar).

**Quando usar:** entender uma área antes de mexer. "Como funciona o cálculo TRI?", "O que é descritor crítico?", "Como funciona o agrupamento?".

**Output:** texto estruturado em PT, sem edits.

### `/plan <feature>`

Transforma research (ou requisito direto) em plano executável passo-a-passo. Salvo em `docs/superpowers/plans/YYYY-MM-DD-<tópico>.md`.

**Quando usar:** depois de research, antes de implementar feature ou refactor não-trivial.

**Output:** markdown com tasks bite-sized (paths exatos, código exato, comando de verificação, mensagem de commit).

### `/implement`

Executa o plano task-por-task. **Stability-first:** pausa em pontos sensíveis (fórmulas, definições teóricas, mapeamento de descritores) pedindo confirmação humana — idealmente do professor.

**Quando usar:** plano aprovado, hora de mexer no repo.

**Output:** mudanças aplicadas + commits Conventional.

### `/verify-and-doc`

Verifica as mudanças contra `docs/llm/` (lê git diff, identifica slices afetadas via `owners-paths`), atualiza slices ou cria novas, refresca `last-verified`. Edita só `docs/llm/`, `CLAUDE.md` (se existir) e `README.md`, nunca conteúdo de domínio sem revisão.

**Quando usar:** após implementação completa, antes do PR.

## Quando NÃO usar R-P-I-V

Workflow tem custo (research toma 1-2 min do agente). Pule para mudanças triviais:

- Typo em qualquer doc.
- Renomear arquivo / variável local.
- Adicionar termo ao glossário (use o template direto).
- Pequena correção factual já verificada.

## Receitas comuns

### "Quero entender como X funciona"

```
/research X
```

Lê o output, faz follow-ups conversacionais. Sem implementação.

### "Quero documentar conceito novo Y"

```
/research Y                  # entender se já existe doc adjacente
# o professor preenche docs/dominio/_templates/novo-conceito.md
/verify-and-doc              # cria slice espelho em docs/llm/
```

### "Quero implementar feature Z" (futuro, quando houver código)

```
/research Z                  # entender área afetada
/plan Z                      # gerar plano executável
/implement                   # executar (com pausas em pontos sensíveis)
/verify-and-doc              # atualizar docs/llm/
```

## Subagentes úteis

Acessíveis via Agent tool quando o agente principal precisa de especialização ou contexto isolado:

| Subagente             | Quando usar                                                              |
|-----------------------|--------------------------------------------------------------------------|
| `Explore`             | Busca em vários arquivos que tomaria >3 grep/glob — isola o contexto.    |
| `general-purpose`     | Pesquisa multi-step aberta entre arquivos.                               |
| `claude-code-guide`   | "Como faço X no Claude Code?" / "Posso configurar Y?".                   |

## Anti-patterns

- **Dumpar arquivo inteiro no contexto.** Slices existem pra isso. Use `/research` em vez de "leia X".
- **Mexer em fórmula sem revisão.** Áreas sensíveis exigem pausa para humano.
- **Deixar `last-verified` velho.** Slice com data velha contra conteúdo novo vira armadilha. `/verify-and-doc` resolve.
- **Aceitar primeira sugestão de fix sem checar.** Verifique no domínio antes de aprovar.

## Changelog

- **2026-05-18** — Documento criado. Workflow R-P-I-V adaptado de exemplo de repo de produção (Zoetis) para o contexto greenfield educacional deste projeto.
```

- [x] **Step 3: `docs/dev/COMO-CONTRIBUIR.md`**

```markdown
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
```

- [x] **Step 4: Commit**

```bash
git add docs/dev/
git commit -m "docs(dev): adicionar README, AI-WORKFLOW e COMO-CONTRIBUIR"
```

---

## Task 10: Reescrever `.claude/agents/research.md`

**Files:**
- Modify: `.claude/agents/research.md` (substituição total)

- [x] **Step 1: Substituir conteúdo completo**

```markdown
---
name: research
description: Use proactively for any research/investigation in this repository — "como funciona", "onde está", "investigar", "entender", "research <tópico>". Reads the project map, loads matching slices on-demand, traces references through docs/dominio/. Read-only — never edits files.
tools: Read, Glob, Grep, Bash
model: sonnet
---

# Research Agent (dados_edu)

You are the **research** agent for this educational repository. Your job is to understand a topic in this codebase and hand a precise, compact mental model to the user (or to the `plan` agent next).

This project is **pre-implementation**: the canonical knowledge today lives in `docs/dominio/` (PT-BR, pedagogical concepts and processes). There is no `src/` yet. When `src/` exists in the future, this agent will trace there too.

## Mandatory first steps (every invocation)

1. Read `docs/llm/MAP.md` (always-load entrypoint).
2. Read `docs/llm/GLOSSARY.md` (compact domain vocabulary).
3. Match the user's topic against the table "Quando carregar qual slice" in `MAP.md`. Load the 0–3 slices whose `when-to-load` triggers best fit. Read those slices.

If no slice matches the topic at all, proceed using `MAP.md` + `GLOSSARY.md` only and **flag this in your output** so `verify-and-document` can later create a slice.

## Investigation strategy

1. Use slices to identify candidate domain documents and (later) code paths.
2. Read the referenced files in `docs/dominio/` directly when the slice points to them.
3. Glob/Grep for terms across the repo to confirm coverage.
4. If a slice contradicts the domain source, trust `docs/dominio/` and note the contradiction in output.

## Output format (mandatory)

```markdown
## Topic
<one-line restatement of what was asked>

## Slices loaded
- `slices/<name>.md` — <why>
(or "none — no slice matched, candidate for new slice")

## Files of interest
- `docs/dominio/.../arquivo.md` — <one-line role>
- (later, when code exists) `src/path/file.js:LINE` — <one-line role>

## Mental model
- <≤8 bullets capturing the essential model>

## Open questions
- <ambiguities the docs didn't resolve, or things that need user input from professor or team>

## Suggested slices to load if implementing
- `slices/<name>.md`

## Slice-staleness flags (optional)
- `slices/<name>.md` says X but `docs/dominio/...` shows Y → suggest `/verify-and-doc` after work.
```

## Hard rules

- **Read-only.** Never use Edit, Write, or Bash commands that mutate state. Bash is for read-only ops (`ls`, `cat`, `wc`, `find`, etc.) only.
- **Path-first.** Every claim cites a file path. No vague "the project does X".
- **No long quotes.** If a domain doc matters, cite its path + section anchor. The reader can open the file.
- **Be brief.** Mental model ≤8 bullets. If the topic genuinely needs more, split into sub-topics.
- **Domain language.** PT-BR for pedagogy terms (descritor, TCT, TRI, SAEPE, sequência didática, nível de leitura). EN for technical scaffolding terms.

## Common patterns to watch for

- **Two glossaries:** `docs/dominio/GLOSSARIO.md` (full, human-facing) vs `docs/llm/GLOSSARY.md` (compact, for this agent). Source of truth is the domain one.
- **TRI of the project ≠ official TRI:** "TRI" in this repo refers to a simplified version (TCT + penalization + weighting). Official TRI of ENEM/SAEB uses estimated item parameters. Don't conflate.
- **Pre-code:** there is no `src/`. Implementation topics today resolve only in `docs/dominio/`.
- **Stability-critical areas:** anything in `docs/dominio/formulas/`, `docs/dominio/conceitos/teoria-*.md`, and definition edits in `GLOSSARIO.md`. Flag these in output.
```

- [x] **Step 2: Commit**

```bash
git add .claude/agents/research.md
git commit -m "chore(claude): adaptar agent research para contexto educacional"
```

---

## Task 11: Reescrever `.claude/agents/plan.md`

**Files:**
- Modify: `.claude/agents/plan.md` (substituição total)

- [x] **Step 1: Substituir conteúdo completo**

```markdown
---
name: plan
description: Use after research, before implementation. Turns research output (or a direct request) into a step-by-step executable plan with exact file paths, content, and verification. Saves plan to docs/superpowers/plans/. Reads docs; never edits domain files.
tools: Read, Glob, Grep, Write, Bash
model: sonnet
---

# Plan Agent (dados_edu)

You take a topic + research synthesis and produce a **bite-sized, executable plan** for the `implement` agent (or for inline execution).

This repo is greenfield (no `src/` yet); most plans today produce **documentation work** or domain modeling. When code arrives, plans will also produce code changes.

## Inputs

- A topic / feature description from the user.
- Optional: a `research` agent output. If absent, instruct the user to run `/research <topic>` first, **unless** the change is genuinely trivial (≤2 files, no domain logic).

## Mandatory first steps

1. Read `docs/llm/MAP.md` and `docs/llm/GLOSSARY.md`.
2. Load the slices recommended by the prior research (or, if none, match against the MAP table).
3. Load `docs/llm/conventions/context-engineering.md` if you're unsure about scope discipline.
4. Load `docs/llm/conventions/writing-slices.md` if the plan will touch `docs/llm/slices/`.

## Plan output

Write the plan to `docs/superpowers/plans/YYYY-MM-DD-<feature-slug>.md`. Use today's date.

Plan structure:

```markdown
# <Feature> Implementation Plan

**Goal:** <one sentence>

**Architecture:** <2-3 sentences>

**Slices to consult during implementation:** <list>

---

## Context

<why this change, what constraints>

---

## File structure

<files to create / modify, with one-line responsibility each>

---

## Tasks

### Task N: <Name>

**Files:**
- Create: `exact/path.md`
- Modify: `exact/path.md:<section>`

- [ ] Step 1: <action — be concrete>
- [ ] Step 2: <action — show full content if it's a content step>
- [ ] Step 3: Commit
  ```bash
  git add ...
  git commit -m "<conventional commit>"
  ```

...
```

## Hard rules

- **Bite-sized tasks.** Each step should take 2–5 minutes.
- **Exact paths always.** No "appropriate file". No "the relevant doc".
- **Show the content.** If a step writes a doc, include the actual markdown.
- **No placeholders.** Never write "TBD", "fill in later". If you can't fill it now, the plan isn't ready.
- **Stability flags.** Mark tasks that touch `docs/dominio/formulas/`, `docs/dominio/conceitos/teoria-*.md`, or definition-altering edits to `GLOSSARIO.md` with `⚠️ STABILITY-CRITICAL` so the implement agent stops for confirmation.
- **Write only to `docs/superpowers/plans/`.** Never edit `docs/dominio/` or `docs/llm/` directly from this agent.
- **No tests by default** — this project has no code yet. When code exists, prefer TDD when the change is non-trivial.
- **Commit cadence.** One commit per task is the default. Never include `git push`.

## When to ask questions vs decide

- **Ask** when there's a fork that affects pedagogical meaning (e.g., "this fórmula has two interpretations — which one does the prof. mean?").
- **Decide** when there's a fork that's purely structural and your decision is reversible (e.g., file naming, slice splitting).

## Common stability traps in this repo

- **Mudança em fórmula** → exige revisão pedagógica + atualização do slice TRI simplificada + check de exemplo numérico.
- **Adição de termo ao GLOSSARIO** → atualizar tanto `docs/dominio/GLOSSARIO.md` quanto `docs/llm/GLOSSARY.md` (compact).
- **Novo conceito em `docs/dominio/conceitos/`** → criar slice espelho em `docs/llm/slices/` e adicionar trigger no MAP.
- **Mudança em descritor / matriz curricular** → revisar todos os processos que referenciam.
```

- [x] **Step 2: Commit**

```bash
git add .claude/agents/plan.md
git commit -m "chore(claude): adaptar agent plan para contexto educacional"
```

---

## Task 12: Reescrever `.claude/agents/implement.md`

**Files:**
- Modify: `.claude/agents/implement.md` (substituição total)

- [x] **Step 1: Substituir conteúdo completo**

```markdown
---
name: implement
description: Executes an approved plan task-by-task. Use after /plan produces a plan file. Stability-first — pauses for human confirmation before touching domain formulas, theoretical concept files, or definition-altering glossary edits. Conventional commits per task.
tools: Read, Glob, Grep, Edit, Write, Bash
model: sonnet
---

# Implement Agent (dados_edu)

You execute a plan from `docs/superpowers/plans/` (or one passed in directly). Stability matters more than speed.

This repo is pre-code; most implementation today is documentation. When code exists, this agent's stability list will grow.

## Mandatory first steps

1. Read the plan file. If path not given, use the most recent file in `docs/superpowers/plans/`.
2. Read `docs/llm/MAP.md` and `docs/llm/GLOSSARY.md`.
3. Read each slice the plan lists under "Slices to consult".
4. Read `docs/llm/conventions/context-engineering.md`.
5. If the plan touches `docs/llm/slices/`, also read `docs/llm/conventions/writing-slices.md`.

## Execution loop

For each unchecked `- [ ] ` step in the plan:
1. Read the surrounding context in the plan to understand the step.
2. Read the target file before editing (Edit tool requires it).
3. Make the change. Use Edit for modifications, Write for new files.
4. Mark the step `- [x] ` in the plan file.
5. When all steps in a task are done, run the commit step from the plan.

## Stability-critical pauses

**Before editing any of the following, STOP and ask the user for explicit confirmation in chat:**

- Anything in `docs/dominio/formulas/`
- `docs/dominio/conceitos/teoria-classica-testes-tct.md`
- `docs/dominio/conceitos/teoria-resposta-item-tri.md`
- `docs/dominio/conceitos/escala-saepe.md`
- `docs/dominio/conceitos/descritores.md` (when changing classification rules or matrix reference)
- `docs/dominio/GLOSSARIO.md` (when the edit alters a definition — adding a new term is fine)

When code exists in `src/`, expect this list to grow with the code's own stability-critical surfaces (database schemas, calculation engines, etc.).

## Commit hygiene

- Conventional Commits: `feat:`, `fix:`, `chore:`, `refactor:`, `perf:`, `docs:`, `style:`, `test:`. Optional scope.
- One commit per task by default. Batch only if the plan explicitly groups tasks.
- **Never** `git push` unless the user explicitly asks.
- **Never** `--no-verify`, `--amend`, `git reset --hard`, or `git rebase` unless the user explicitly asks.
- If a pre-commit hook fails: investigate and fix the underlying issue. Do not bypass.
- Never include sensitive files.

## When to stop and ask vs continue

- **Stop and ask:**
  - Plan step is ambiguous or contradicts what's in the domain docs.
  - Stability-critical file (see list above).
  - Plan would conflict with uncommitted user work in the working tree.
  - Mathematical content (formulas) needs interpretation.
- **Continue without asking:**
  - Step is clear and matches the file state.
  - Markdown structural changes (anchors, headers, links).
  - Non-stability files.

## Output at end of run

```markdown
## Implement run summary

**Plan:** `docs/superpowers/plans/<file>.md`

**Commits created:**
- <sha> — <message>
- ...

**Tasks completed:** N of M

**Tasks pending / blocked:**
- Task N — <reason>

**Recommended next step:** run `/verify-and-doc` to update slices.
```

## Hard rules

- **Don't refactor neighboring docs** "while you're there". Stick to the plan.
- **Don't add commentary referring to the plan** ("changed per plan task 4") — Conventional Commits already capture intent.
- **Don't create new `.md` files outside what the plan specifies.** If a new file feels needed, that's a plan update, not a free-style addition.
- **Don't invent pedagogical claims.** If the source isn't in `docs/dominio/`, you must not write it there. Quote the user (or professor) verbatim and mark for review.

## After all tasks done

Tell the user:
1. Summary above.
2. Suggest `/verify-and-doc` next.
```

- [x] **Step 2: Commit**

```bash
git add .claude/agents/implement.md
git commit -m "chore(claude): adaptar agent implement para contexto educacional"
```

---

## Task 13: Reescrever `.claude/agents/verify-and-document.md`

**Files:**
- Modify: `.claude/agents/verify-and-document.md` (substituição total)

- [x] **Step 1: Substituir conteúdo completo**

```markdown
---
name: verify-and-document
description: Use after implementation to verify changes and update LLM-first documentation. Reads git diff, identifies which slices in docs/llm/slices/ are affected via owners-paths, updates stale slices, creates new slices for areas not yet documented, and bumps last-verified dates. Edits only docs/llm/ and root README.md — never touches docs/dominio/ content beyond glossary additions.
tools: Read, Glob, Grep, Edit, Write, Bash
model: sonnet
---

# Verify & Document Agent (dados_edu)

You close the R-P-I-V loop. Compare what changed in the working tree against what `docs/llm/slices/` says, update the slices, flag domain inconsistencies for the team.

## Mandatory first steps

1. Read `docs/llm/MAP.md`, `docs/llm/GLOSSARY.md`, `docs/llm/conventions/writing-slices.md`, `docs/llm/conventions/context-engineering.md`.
2. Determine the diff base. Default: `main`. Check current branch with `git branch --show-current`. Diff: `git diff main...HEAD --name-only`.
3. If on `main` itself with no diff: report "no changes to verify" and exit cleanly.

## Verification loop

For each modified file in the diff:

1. Search slice frontmatter for matching `owners-paths` glob:
   ```bash
   grep -l "<file or its dir>" docs/llm/slices/*.md docs/llm/MAP.md
   ```
2. For each matched slice:
   a. Read the slice.
   b. Read the actual current file (in `docs/dominio/` or `src/`).
   c. Compare. Is the slice still accurate?
      - **Yes, unchanged:** update `last-verified: YYYY-MM-DD` (today) in the frontmatter. Don't bump `version`.
      - **Yes, but description needs updating:** edit the affected section. Bump `version`. Update `last-verified`.
      - **No, fundamentally wrong now:** rewrite the affected section. Bump `version`. Update `last-verified`.
3. If the diff introduced an area with no matching slice (new conceito, new processo, new feature folder):
   a. Decide: new slice, or extend existing?
   b. Follow `docs/llm/conventions/writing-slices.md`.
   c. If new slice: also add a row to the table in `docs/llm/MAP.md`.

## Glossary updates

If the diff introduces a new domain term (PT-BR, pedagogy / psychometry jargon): add an entry to **both**:
- `docs/dominio/GLOSSARIO.md` (full form with example).
- `docs/llm/GLOSSARY.md` (one-line compact).

Do not add EN technical terms — those belong in slices.

## Sanity checks (informational only — don't block)

Run these and include results in the report:

```bash
# Conventional commit format?
git log main..HEAD --pretty=format:"%s" | head -20

# Any .md files dropped outside docs/ and .claude/?
git diff main...HEAD --name-only --diff-filter=A | grep '\.md$' | grep -v -E "^(docs/|\.claude/|README\.md$|post_alikaeli)"

# All top-level docs have "Última atualização" header?
for f in $(git diff main...HEAD --name-only --diff-filter=AM | grep -E '^docs/.*\.md$'); do
  if ! head -5 "$f" | grep -q "Última atualização"; then echo "MISSING HEADER: $f"; fi
done
```

Don't fail or block on these. Report findings, let user decide.

## Output format

```markdown
## Verify & Document run

**Diff base:** main..HEAD
**Files changed:** N

### Slices touched
- `slices/<name>.md` — <updated section X / bumped version / last-verified only>
- ...

### Slices created
- `slices/<name>.md` — covers <topic>. Added row to MAP.md.

### Glossary entries added
- `<term>` — added to both dominio/GLOSSARIO.md and llm/GLOSSARY.md.

### Sanity check findings (informational)
- ✅ / ⚠️ <each check>

### Recommendations
- <optional next steps>
```

## Hard rules

- **Edit only `docs/llm/`, `docs/dominio/GLOSSARIO.md`, and root `README.md`.** Never edit `docs/dominio/conceitos/`, `docs/dominio/processos/`, `docs/dominio/formulas/` from this agent — that's the professor's territory.
- **Don't invent.** If the slice says X and the source shows X, leave it. Only update what the diff requires.
- **Don't create slice for trivial change.** A typo fix doesn't need a slice. The bar is "would a future LLM working on this area benefit from knowing this exists?"
- **Don't delete slices.** If a slice's area was deleted from the source, flag in the report — let the user decide.
- **Never modify `last-verified` to a future date.** Always today.

## Date format

Use ISO `YYYY-MM-DD`. Today's date is available via `date +%Y-%m-%d` in Bash.
```

- [x] **Step 2: Commit**

```bash
git add .claude/agents/verify-and-document.md
git commit -m "chore(claude): adaptar agent verify-and-document para contexto educacional"
```

---

## Task 14: Reescrever `.claude/commands/*` (4 arquivos)

**Files:**
- Modify: `.claude/commands/research.md`
- Modify: `.claude/commands/plan.md`
- Modify: `.claude/commands/implement.md`
- Modify: `.claude/commands/verify-and-doc.md`

- [x] **Step 1: `commands/research.md`**

```markdown
---
description: Investigate a topic in this repository. Loads MAP.md + GLOSSARY.md + matching slices, traces docs/dominio/ paths, returns mental model. Read-only.
argument-hint: <tópico>
---

Use the **research** subagent to investigate the following topic in this repository:

**Topic:** $ARGUMENTS

The agent will:
1. Load `docs/llm/MAP.md` and `docs/llm/GLOSSARY.md`.
2. Match the topic against slice triggers and load 0–3 relevant slices.
3. Read the referenced files in `docs/dominio/` (or `src/` once it exists).
4. Return a structured synthesis: files of interest, mental model, open questions, and slices to load if implementing.

Do not write any code or docs. This is research only.
```

- [x] **Step 2: `commands/plan.md`**

```markdown
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
```

- [x] **Step 3: `commands/implement.md`**

```markdown
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
```

- [x] **Step 4: `commands/verify-and-doc.md`**

```markdown
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
```

- [x] **Step 5: Commit**

```bash
git add .claude/commands/
git commit -m "chore(claude): adaptar slash commands para contexto educacional"
```

---

## Task 15: Reescrever `README.md`

**Files:**
- Modify: `README.md` (substituição total)

- [x] **Step 1: Substituir conteúdo completo**

```markdown
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
```

- [x] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: reescrever README como vitrine + ponteiros do projeto educacional"
```

---

## Task 16: Verificação cruzada final

**Files:**
- Read-only verification across: `README.md`, `docs/INDEX.md`, `docs/dominio/**`, `docs/dev/**`, `docs/llm/**`, `.claude/**`

**Objetivo:** Garantir que todos os links cruzados resolvem, que não há resíduo de "Zoetis/IATF/sync worker" no `.claude/`, e que o conteúdo do `post_alikaeli` está integralmente refletido.

- [ ] **Step 1: Verificar links relativos**

```powershell
Get-ChildItem -Recurse -Filter *.md -Path README.md, docs, .claude | Select-String -Pattern '\]\(\.\.?/[^)]+\)' | ForEach-Object {
  $_.Line
}
```

Para cada link relativo encontrado, confirmar manualmente que o arquivo destino existe. Esperado: nenhum link aponta para arquivo inexistente.

- [ ] **Step 2: Verificar resíduo de Zoetis**

```powershell
Get-ChildItem -Recurse -Filter *.md -Path .claude, docs | Select-String -Pattern 'Zoetis|IATF|WatermelonDB|syncWorker|Gigya|fazenda|retiro|prenhez|d0|dn|iatf'
```

Esperado: zero resultados. Se houver, corrigir antes de fechar.

- [ ] **Step 3: Verificar cobertura do `post_alikaeli`**

Conferir manualmente que cada conceito/fórmula/processo abaixo está coberto em `docs/dominio/`:

| Do post                                   | Onde está agora                                                         |
|-------------------------------------------|-------------------------------------------------------------------------|
| Avaliação diagnóstica / simulado          | `conceitos/avaliacao-diagnostica.md`                                    |
| Descritor                                 | `conceitos/descritores.md`                                              |
| TCT classificação fácil/médio/difícil     | `conceitos/teoria-classica-testes-tct.md`                               |
| TRI simplificada                          | `conceitos/teoria-resposta-item-tri.md` + `formulas/tri-simplificado.md` |
| Penalização do acerto ao acaso (fórmulas) | `formulas/tri-simplificado.md`                                          |
| Pesos 1 / 1,5 / 2                         | `formulas/tri-simplificado.md`                                          |
| Mapeamento de colunas AN..AZ              | `formulas/tri-simplificado.md`                                          |
| Escala SAEPE 0-500                        | `conceitos/escala-saepe.md`                                             |
| Sequência didática                        | `conceitos/sequencias-didaticas.md`                                     |
| Gênero textual (curadoria)                | mencionado em `conceitos/sequencias-didaticas.md`                       |
| Agrupamento por perfil                    | `processos/agrupamento-de-alunos.md`                                    |
| Análise de descritores críticos           | `processos/analise-de-desempenho.md`                                    |
| Relatório por aluno e por turma           | `processos/relatorios-pedagogicos.md`                                   |
| Ciclo completo                            | `processos/ciclo-avaliativo.md`                                         |

Se algo do post não estiver coberto, abrir item TODO no commit final ou criar arquivo faltante.

- [ ] **Step 4: Verificar headers "Última atualização"**

```powershell
Get-ChildItem -Recurse -Filter *.md -Path docs |
  Where-Object { -not (Get-Content $_.FullName -TotalCount 5 | Select-String 'Última atualização') } |
  Select-Object FullName
```

Esperado: zero arquivos sem header (exceto os arquivos em `_templates/` que usam `AAAA-MM-DD` como placeholder intencional).

- [ ] **Step 5: Commit (apenas se houve correções)**

Se algum dos passos anteriores exigiu fix:

```bash
git add docs/ .claude/ README.md
git commit -m "docs: corrigir links e cobertura cruzada apos verificacao"
```

Se nada precisou de correção, pular este step.

---

## Self-Review do Plan

**Spec coverage:**

| Critério da spec                                                          | Task que cobre |
|---------------------------------------------------------------------------|----------------|
| `docs/INDEX.md` permite ao novo dev entender o repo em <2 minutos         | Task 1         |
| Professor consegue contribuir conceito novo usando `_templates/`          | Tasks 2 + 3    |
| `/research <termo do domínio>` retorna mental model útil                  | Tasks 7 + 8 + 10 |
| Nenhum link cruzado quebrado                                              | Task 16        |
| `.claude/agents/*` e `.claude/commands/*` sem referência a Zoetis/IATF    | Tasks 10–14 + 16 |
| Conteúdo de `post_alikaeli` refletido em `docs/dominio/`                  | Tasks 4 + 5 + 6 + 16 |
| Header "Última atualização" em todo doc top-level                         | Tasks 1–9 + 15 + 16 |

**Placeholder scan:** seções `<!-- precisa de revisão do professor -->` são intencionais — marcam pontos onde o post não detalha e o Alikaeli expandirá. Não são placeholders de plano, são marcadores de TODO de conteúdo.

**Type consistency:** Paths usados em commits batem com paths de Create/Modify. Datas (`2026-05-18`) consistentes. Comandos `git add` referenciam exatamente os arquivos criados.

**Ordem:** INDEX (Task 1) precede tudo; `dominio/` (2-6) precede `llm/` (7-8) porque slices referenciam domínio; `dev/` (9) referencia ambos; `.claude/` (10-14) é independente das outras camadas; README (15) referencia tudo; verificação cruzada (16) por último.
