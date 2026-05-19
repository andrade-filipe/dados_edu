# Skills do Projeto Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Criar 5 skills project-level em `.claude/skills/` (`documentar-conceito`, `documentar-processo`, `atualizar-glossario`, `revisar-marcadores-todo`, `evoluir-tooling`) e ajustar o agent `verify-and-document` para recomendar `evoluir-tooling` quando detectar drift.

**Architecture:** Skills são arquivos `.md` com frontmatter (`name`, `description`) seguido de corpo estruturado em "Quando ativar / Mandatory first steps / Workflow / Hard rules / Output". 4 das 5 são voltadas a workflow professor↔dev em domínio; a 5ª é meta-tooling.

**Tech Stack:** Markdown puro. Sem dependências.

**Spec:** [`docs/superpowers/specs/2026-05-18-skills-projeto-design.md`](../specs/2026-05-18-skills-projeto-design.md)

**Ordem de execução:** criar diretório → 5 skills (independentes entre si) → ajuste no agent `verify-and-document` → verificação cruzada.

**Convenção:** todos os commits seguem Conventional Commits sem acentos (`feat:`, `chore:`, `docs:`).

**Stability-critical:** Task 7 (ajuste em `verify-and-document`) — pausar antes de aplicar; mudança em agent tem efeito multiplicador.

---

## Task 1: Criar `.claude/skills/documentar-conceito.md`

**Files:**
- Create: `.claude/skills/documentar-conceito.md`

- [x] **Step 1: Criar arquivo com conteúdo completo**

```markdown
---
name: documentar-conceito
description: Use quando o professor quer documentar um conceito pedagógico ou psicométrico novo (TCT, TRI, descritor, gênero textual, nível de leitura, etc.) e ainda não há arquivo em docs/dominio/conceitos/ para esse tema. Triggers PT-BR: "documentar conceito", "escrever sobre [X]", "criar arquivo em conceitos", "preencher template de conceito".
---

# Documentar Conceito (skill PT-BR para o professor)

Esta skill conduz o professor (ou um dev colaborando com ele) na criação de um arquivo em `docs/dominio/conceitos/`, preenchendo o template `_templates/novo-conceito.md` por meio de perguntas guiadas em PT-BR. Objetivo: capturar a voz do professor sem exigir que ele edite markdown diretamente.

## Quando ativar

- Conversa contém pedido explícito ("vamos documentar X", "transforma isso em conceito", "criar arquivo em conceitos").
- Professor está explicando algo pedagógico/teórico de forma narrativa e seria útil capturar como conceito reutilizável.
- Pedido de revisão de conceito existente → **NÃO** é esta skill, é `revisar-marcadores-todo` ou edição direta.

## Mandatory first steps

1. Ler `docs/dominio/_templates/novo-conceito.md` para conhecer a estrutura exata.
2. Listar `docs/dominio/conceitos/` para evitar duplicata e ler 1-2 arquivos como referência de estilo (preferência por `teoria-classica-testes-tct.md` ou `descritores.md` — bons exemplos).
3. Ler `docs/dominio/GLOSSARIO.md` para conhecer o vocabulário já registrado.

## Workflow

1. **Confirmar o nome.** Perguntar: "Qual é o nome do conceito que vamos documentar?" Derivar o slug em kebab-case (acentos viram letra simples, espaços viram hífen). Mostrar o slug proposto e o path final: `docs/dominio/conceitos/<slug>.md`. Confirmar.

2. **Checar duplicata.** Se o arquivo já existe, dizer ao prof e perguntar se quer (a) editar o existente, (b) escolher outro nome, ou (c) cancelar.

3. **Coletar seção por seção.** Para cada seção do template, fazer uma pergunta em PT-BR. Use as perguntas abaixo (adapte sutilmente, não recite robótico):

   - **O que é** → "Em uma ou duas frases simples, como você explicaria esse conceito para um colega de outra área?"
   - **Por que importa para o nosso projeto** → "Em que momento do projeto esse conceito aparece? Por que precisamos dele?"
   - **Como funciona** → "Pode me explicar como esse conceito funciona, com o nível de detalhe que achar necessário?"
   - **Exemplo concreto** → "Você lembra de um exemplo real (de turma, de prova, de aluno) que ilustra esse conceito? Pode anonimizar nomes."
   - **Limites e cuidados** → "O que esse conceito NÃO faz? Onde alguém pode se confundir?"
   - **Relação com outros conceitos** → "Esse conceito se conecta com algum outro do projeto? Quais?" (cruzar com a listagem de `conceitos/`)
   - **Referências** → "Tem algum livro, artigo, documento oficial (BNCC, INEP, SAEPE) que apoia essa definição?" (opcional — pode ficar vazio)

4. **Preservar a voz do professor.** Não reescrever as respostas — no máximo corrigir gramática óbvia ou quebrar parágrafo. Se a resposta veio em bullet, manter bullet.

5. **Detectar termos novos.** Durante a coleta, anotar qualquer termo de domínio que apareceu e não está no `GLOSSARIO.md`. Ao final, sugerir: "Identifiquei os termos X, Y, Z que ainda não estão no glossário. Quer adicioná-los agora?" → se sim, encadear `atualizar-glossario`.

6. **Montar e mostrar.** Construir o markdown final usando o template como esqueleto. Mostrar ao professor antes de gravar e perguntar: "Está bom assim? Quer ajustar alguma seção?"

7. **Gravar.** Write em `docs/dominio/conceitos/<slug>.md`.

8. **Sugerir próximos passos:**
   - "Recomendo rodar `/verify-and-doc` para criar a slice LLM espelho desse conceito."
   - Se houve termos novos pendentes, lembrar do `atualizar-glossario`.
   - Sugerir commit: `docs(dominio): adicionar conceito <nome>`.

## Hard rules

- **Não inventar conteúdo pedagógico.** Se o professor não souber ou não quiser preencher uma seção, gravar `<!-- precisa de revisão do professor: <pergunta específica> -->` e seguir.
- **Não pular seções "para acelerar".** O template é o template; toda seção é perguntada.
- **Áreas sensíveis** (conceitos relacionados a TCT, TRI, SAEPE, fórmulas): além das perguntas normais, mostrar o conteúdo coletado e perguntar de novo "isso aqui está pedagogicamente correto?" antes de gravar.
- **Slug sem acento, kebab-case, minúsculo.** "Análise de Variância" → `analise-de-variancia.md`.
- **Header de atualização obrigatório:** primeira linha do corpo após o `#` deve ser `> Última atualização: <hoje YYYY-MM-DD> · Mantenedor: prof. Alikaeli` (ajustar mantenedor se for outro).
- **Não criar arquivo em `_templates/` ou em outro subdir** — só em `docs/dominio/conceitos/`.

## Output ao final da skill

```markdown
## Conceito documentado

**Arquivo:** docs/dominio/conceitos/<slug>.md
**Nome:** <Nome do conceito>
**Seções marcadas para revisão:** <lista de seções deixadas com TODO, se houver>
**Termos novos identificados:** <lista, se houver>

**Sugestões de próximo passo:**
- [ ] `atualizar-glossario` para registrar termos novos.
- [ ] `/verify-and-doc` para criar slice LLM.
- [ ] Commit: `docs(dominio): adicionar conceito <slug>`
```
```

- [x] **Step 2: Commit**

```bash
git add .claude/skills/documentar-conceito.md
git commit -m "feat(skills): adicionar skill documentar-conceito para professor"
```

---

## Task 2: Criar `.claude/skills/documentar-processo.md`

**Files:**
- Create: `.claude/skills/documentar-processo.md`

- [x] **Step 1: Criar arquivo com conteúdo completo**

```markdown
---
name: documentar-processo
description: Use quando o professor quer documentar um processo de trabalho (como faz hoje, em que ferramenta, com que artefato, onde dói). Triggers PT-BR: "documentar processo", "escrever sobre como faço [X]", "criar arquivo em processos", "registrar fluxo de trabalho".
---

# Documentar Processo (skill PT-BR para o professor)

Análoga à `documentar-conceito`, mas focada em **processos** — sequências concretas de trabalho que o professor executa hoje. As seções "Pontos de dor" e "O que seria ideal" são tratadas com atenção especial porque alimentam diretamente o design técnico futuro.

## Quando ativar

- Pedido explícito ("vamos documentar como faço a coleta de dados", "registrar o processo de aplicação de simulado").
- Professor descreve fluxo de trabalho narrativamente e seria útil capturar como processo reutilizável.
- Se for **conceito teórico** e não processo de trabalho → use `documentar-conceito`.

## Mandatory first steps

1. Ler `docs/dominio/_templates/novo-processo.md`.
2. Listar `docs/dominio/processos/` e ler 1-2 arquivos como referência (preferência: `ciclo-avaliativo.md` ou `coleta-de-dados.md`).
3. Ler `docs/dominio/GLOSSARIO.md` para vocabulário comum.

## Workflow

1. **Confirmar o nome.** "Qual é o nome desse processo?" Slug em kebab-case. Path: `docs/dominio/processos/<slug>.md`. Confirmar.

2. **Checar duplicata** (igual a `documentar-conceito`).

3. **Coletar seção por seção:**

   - **Objetivo do processo** → "Qual é o resultado final desse processo? O que ele entrega?"
   - **Quando ele acontece** → "Em que momento do ano letivo / bimestre / semana esse processo acontece? Com que frequência?"
   - **Pré-requisitos** → "O que precisa estar pronto antes de começar?"
   - **Passo a passo** → coletar **um passo de cada vez**. Pergunta inicial: "Qual é o primeiro passo?" Depois: "E depois? Qual o próximo passo?" Repetir até o professor dizer "acabou" ou similar. Numerar automaticamente.
   - **Artefatos produzidos** → "O que sai desse processo? Planilhas, relatórios, decisões, agrupamentos?"
   - **Pontos de dor** → ⚠️ **insistir** aqui. Pergunta principal: "O que mais dói nesse processo hoje? O que toma muito tempo? O que erra com frequência?" Se a resposta for seca, perguntar: "Tem algum passo que você adia porque é chato? Algum que você gostaria de não precisar fazer?"
   - **O que seria ideal** → ⚠️ **não pode ficar vazio**. Pergunta principal: "Se você pudesse apertar um botão e o resto saísse pronto, o que esse botão faria?" Se o prof não souber, perguntar: "Quais decisões você ainda gostaria de tomar manualmente, mesmo num cenário ideal?"
   - **Conceitos relacionados** → "Quais conceitos do projeto esse processo usa?" (cruzar com `docs/dominio/conceitos/`)

4. **Preservar a voz.** Mesma regra de `documentar-conceito`.

5. **Detectar termos novos** (igual `documentar-conceito`).

6. **Montar e mostrar.** Confirmar com o prof.

7. **Gravar** em `docs/dominio/processos/<slug>.md`.

8. **Sugerir próximos passos** (igual `documentar-conceito`, ajustando para "processo").

## Hard rules

- **"O que seria ideal" não pode ficar vazio.** Se o prof não tem opinião agora, gravar `<!-- precisa de revisão do professor: o que esse processo automatizado faria? -->` com pergunta concreta.
- **Passos numerados** mesmo que o prof diga em texto corrido — converter para 1., 2., 3. ao gravar.
- **Não inventar** dor onde o prof não disse. Se ele diz "nada dói", gravar isso e prosseguir.
- **Slug, header de atualização, áreas sensíveis:** mesmas regras de `documentar-conceito`.

## Output

```markdown
## Processo documentado

**Arquivo:** docs/dominio/processos/<slug>.md
**Nome:** <Nome do processo>
**Pontos de dor identificados:** <N>
**Características do "ideal":** <resumo de 1 linha>
**Seções marcadas para revisão:** <lista, se houver>

**Sugestões de próximo passo:**
- [ ] `atualizar-glossario` para termos novos.
- [ ] `/verify-and-doc` para criar slice LLM.
- [ ] Commit: `docs(dominio): documentar processo <slug>`
```
```

- [x] **Step 2: Commit**

```bash
git add .claude/skills/documentar-processo.md
git commit -m "feat(skills): adicionar skill documentar-processo para professor"
```

---

## Task 3: Criar `.claude/skills/atualizar-glossario.md`

**Files:**
- Create: `.claude/skills/atualizar-glossario.md`

- [x] **Step 1: Criar arquivo com conteúdo completo**

```markdown
---
name: atualizar-glossario
description: Use para adicionar novo termo PT-BR de domínio aos dois glossários (dominio/GLOSSARIO.md completo + llm/GLOSSARY.md compacto) de forma atômica. Triggers PT-BR: "adicionar ao glossário", "registrar termo", "novo termo", "atualizar glossário". Também acionada por verify-and-document quando detecta termo novo num diff.
---

# Atualizar Glossário (skill atômica para os dois glossários)

Esta skill garante que adicionar um termo de domínio sempre atualiza **ambos** os glossários consistentemente — `docs/dominio/GLOSSARIO.md` (completo, voz humana) e `docs/llm/GLOSSARY.md` (compacto, voz de agente). Atomicidade é o ponto: se uma gravação falhar, a outra é revertida.

## Quando ativar

- Pedido explícito ("adicionar ao glossário", "registrar termo Y").
- Durante `documentar-conceito` ou `documentar-processo`, detectado termo novo.
- Durante `verify-and-document`, diff de domínio introduz termo PT-BR ainda não glossado.

## Mandatory first steps

1. Ler `docs/dominio/GLOSSARIO.md` completo (estrutura: ordem alfabética, cada termo com `## <Termo>` + `**Definição:**` + `**Exemplo de uso:**` + `**Referência:**` opcional, separados por `---`).
2. Ler `docs/llm/GLOSSARY.md` completo (estrutura: tabela `| Termo | Definição compacta |`).

## Workflow

1. **Receber o termo.** Se chamada explicitamente, perguntar "Qual termo vamos adicionar?". Se via outra skill, receber via contexto.

2. **Normalizar para comparação.** Lowercase + remover acentos para checagem de duplicata (mas preservar grafia original para gravação).

3. **Checar duplicata.** Buscar em ambos os glossários.
   - Se existe em **ambos** com mesma grafia → mostrar entrada atual e perguntar: "Esse termo já existe. Quer atualizar a definição (sim/não)?"
     - Se sim → seguir como update.
     - Se não → encerrar.
   - Se existe em **apenas um** → reportar inconsistência: "Termo está só em <arquivo>. Vou sincronizar antes de adicionar." Seguir com sync + add.

4. **Coletar conteúdo.** Perguntar em PT-BR:
   - "**Definição** (uma frase clara — como você diria pra um colega)?"
   - "**Exemplo de uso** (uma frase em contexto)?"
   - "**Referência** (BNCC, INEP, SAEPE, livro… ou pule)?"

5. **Validar.**
   - Definição ≤300 caracteres? Se passou, perguntar "essa definição é longa — quer encurtar ou tudo bem?".
   - Versão compacta para o LLM: derivar uma linha ≤120 caracteres a partir da definição. Mostrar e confirmar: "Para o glossário do agente vai assim: `<linha compacta>` — ok?"

6. **Encontrar posição alfabética.**
   - Em `dominio/GLOSSARIO.md`: identificar o termo imediatamente anterior e posterior na ordem.
   - Em `llm/GLOSSARY.md`: mesma posição na tabela.

7. **Montar diffs.** Preparar os dois edits.

   `dominio/GLOSSARIO.md` (insert entre `<anterior>` e `<posterior>`):
   ```markdown
   ## <Termo>

   **Definição:** <definição completa>

   **Exemplo de uso:** "<exemplo>"

   **Referência:** <ref> (omitir linha se não houver)

   ---
   ```

   `llm/GLOSSARY.md` (insert na tabela):
   ```markdown
   | <Termo> | <linha compacta>. |
   ```

8. **Mostrar ambos os diffs antes de gravar.** "Vou gravar isso. Confirma?"

9. **Gravar atomicamente:**
   - Edit `dominio/GLOSSARIO.md`.
   - Edit `llm/GLOSSARY.md`.
   - Se a segunda falhar, reverter a primeira (Edit reverso).

10. **Sugerir commit:**
    ```
    docs(dominio): adicionar termo <Termo> ao glossario
    ```

## Hard rules

- **Atomicidade dos dois glossários.** Falha num → reverter outro. Nunca deixar termo órfão.
- **Grafia consistente.** Acentos, hífens, capitalização idênticos nos dois arquivos.
- **Compacto ≤120 caracteres.** Se a definição completa for curta, copiar; se for longa, perguntar ao usuário como reduzir.
- **Ordem alfabética rigorosa.** Considere acentos pela letra base (`á` ordena como `a`).
- **Atualização ≠ adição.** Em update, manter o `---` separador e os campos opcionais.
- **Nunca alterar definição existente sem aviso.** Se usuário quer mudar significado, perguntar duas vezes (mudança em glossário muda interpretação de outros docs).

## Output

```markdown
## Glossário atualizado

**Termo:** <Termo>
**Operação:** adicionado / atualizado
**Arquivos modificados:**
- `docs/dominio/GLOSSARIO.md` (linha N — inserido entre <anterior> e <posterior>)
- `docs/llm/GLOSSARY.md` (linha M — inserido na tabela)

**Sugestão de commit:** `docs(dominio): adicionar termo <Termo> ao glossario`
```
```

- [x] **Step 2: Commit**

```bash
git add .claude/skills/atualizar-glossario.md
git commit -m "feat(skills): adicionar skill atualizar-glossario com atomicidade dos dois glossarios"
```

---

## Task 4: Criar `.claude/skills/revisar-marcadores-todo.md`

**Files:**
- Create: `.claude/skills/revisar-marcadores-todo.md`

- [x] **Step 1: Criar arquivo com conteúdo completo**

```markdown
---
name: revisar-marcadores-todo
description: Use quando o professor quer percorrer e resolver marcadores `<!-- precisa de revisão do professor -->` que ficaram espalhados pela documentação do domínio. Triggers PT-BR: "revisar todos", "preencher marcadores", "varrer pendências", "limpar TODOs do domínio".
---

# Revisar Marcadores TODO (skill PT-BR para o professor)

Esta skill conduz o professor através dos `<!-- precisa de revisão do professor -->` em `docs/dominio/`, um por vez, capturando a resposta e atualizando o arquivo.

## Quando ativar

- Pedido explícito de revisão em lote.
- Início de sessão dedicada com o professor.
- Após `documentar-conceito` ou `documentar-processo` que deixou múltiplos marcadores.

## Mandatory first steps

1. Rodar busca:
   ```bash
   grep -rn "precisa de revisão do professor" docs/dominio/
   ```
2. Estruturar resultado como lista:
   - `<arquivo>:<linha> · <heading da seção onde está o marcador> · <pergunta específica do marcador, se houver>`
3. Contar total. Mostrar ao prof: "Encontrei N marcadores espalhados em M arquivos. Quer revisar agora?"
4. Se prof aceitar, ler `docs/dominio/_templates/novo-conceito.md` e `_templates/novo-processo.md` para conhecer as perguntas-tipo de cada seção (vai ajudar a contextualizar).

## Workflow

1. **Apresentar resumo.** Listar marcadores agrupados por arquivo. Perguntar: "Quer começar por algum arquivo específico ou seguir a ordem que listei?"

2. **Para cada marcador, em ordem:**

   a. **Abrir o arquivo na seção.** Read tool com offset/limit para mostrar:
      - 1 linha do heading imediatamente acima.
      - O parágrafo onde o marcador está.
      - 2-3 linhas depois.

   b. **Mostrar o contexto.** Bloco curto com path + heading + trecho.

   c. **Pergunta principal.** Em PT-BR, adaptada ao conteúdo:
      - Se o marcador tem pergunta específica embutida (`<!-- ... pergunta concreta -->`), repassar essa pergunta.
      - Caso contrário: "Aqui o que vai? (Pode dizer 'pular' se ainda não quiser preencher.)"

   d. **Capturar a resposta.**
      - Se prof responde com conteúdo → aplicar Edit removendo o marcador e inserindo a resposta no lugar correto da seção.
      - Se prof diz "pular" → deixar como está, registrar no relatório.
      - Se prof quer renomear o marcador (tornar a pergunta mais específica) → aplicar Edit alterando só o texto do marcador.

   e. **Avançar.** Sem comentário extra — manter ritmo. Próximo marcador.

3. **Pausa cooperativa.** A cada 5 marcadores, perguntar: "Quer continuar ou pausar para retomar depois?"

4. **Áreas sensíveis** (arquivos em `docs/dominio/formulas/` ou `conceitos/teoria-*.md`): pausa extra. "Esse marcador está em área sensível (fórmula/teoria). A resposta vai ser revisada por um segundo professor?" Apenas registrar; não bloquear.

5. **Ao final** (ou em pausa):

   ```markdown
   ## Revisão concluída (ou pausada)

   **Resolvidos:** N de M marcadores
   **Pulados:** K (lista de arquivos + seção)
   **Áreas sensíveis tocadas:** <lista, se houver — recomendar review>

   **Sugestão de commit:**
   `docs(dominio): preencher N marcadores de revisao do professor`
   ```

## Hard rules

- **Um marcador por vez.** Não combinar em formulário gigante.
- **Mostrar contexto suficiente** para o prof entender a seção sem reler o arquivo inteiro.
- **Não bombardear.** Aceitar "pular" e "pausar" sem questionar.
- **Preservar formatação ao redor.** O Edit substitui exatamente o marcador (com comentário HTML) pelo texto novo — não mexer em outras linhas.
- **Áreas sensíveis pedem flag visual** no output final.
- **Não inventar conteúdo.** Se a resposta do prof for "não sei", manter o marcador (talvez reescrito com pergunta mais específica).

## Output (relatório final)

(Conforme item 5 acima.)
```

- [x] **Step 2: Commit**

```bash
git add .claude/skills/revisar-marcadores-todo.md
git commit -m "feat(skills): adicionar skill revisar-marcadores-todo para limpar pendencias do dominio"
```

---

## Task 5: Criar `.claude/skills/evoluir-tooling.md`

**Files:**
- Create: `.claude/skills/evoluir-tooling.md`

- [x] **Step 1: Criar arquivo com conteúdo completo**

```markdown
---
name: evoluir-tooling
description: Use para detectar e aplicar drift entre `.claude/` (agents, commands, skills) e a realidade do projeto. Parceira do agent verify-and-document. Triggers: "melhorar agents", "atualizar skills", "tooling antiquado", "evoluir .claude", "tooling health check".
---

# Evoluir Tooling (skill meta — mantém `.claude/` vivo)

Esta skill detecta drift entre o tooling do projeto (`.claude/agents/`, `.claude/commands/`, `.claude/skills/`) e a realidade atual de `docs/dominio/` e (futuramente) `src/`. Propõe diffs cirúrgicos, aplica após confirmação humana por mudança.

⚠️ **Cada alteração em `.claude/` é stability-critical.** Erro no tooling se propaga para todo o trabalho futuro.

## Quando ativar

- Pedido explícito ("rodar tooling health", "melhorar agents", "atualizar skills do projeto").
- Sugestão do `verify-and-document` ao final de uma rodada de verify (quando ele detecta padrões novos no diff que merecem atenção do tooling).
- Periodicamente (mensal/bimestral), como ritual de manutenção.

## Mandatory first steps

1. Ler tudo em `.claude/`:
   - `.claude/agents/*.md`
   - `.claude/commands/*.md`
   - `.claude/skills/*.md`
2. Ler commits recentes em `docs/dominio/` (últimas 2 semanas por padrão):
   ```bash
   git log --since="2 weeks ago" --name-status --pretty=format:"%h %s" -- docs/dominio
   ```
3. Quando houver `src/`, repetir o log para ele.
4. Ler `docs/llm/MAP.md` (índice de slices e triggers).

## Workflow

1. **Mapear drift por categoria.** Para cada arquivo em `.claude/`, comparar com a realidade. Categorias:

   - **A. Stability-critical desatualizado.**
     - Agent diz "pause em `docs/dominio/formulas/`" mas pasta foi renomeada → drift.
     - Lista de paths sensíveis defasada → drift.

   - **B. Triggers de research ausentes.**
     - Conceito novo em `docs/dominio/conceitos/` não tem trigger correspondente em `agents/research.md` ou `llm/MAP.md` → drift.

   - **C. Paths obsoletos.**
     - Skill / agent referencia arquivo renomeado, movido ou deletado → drift.

   - **D. Skill faltante.**
     - Workflow aparece repetidamente nas últimas conversas (sinal: mesma pergunta resolvida 3+ vezes) e ainda não é skill → candidato.

   - **E. Crosslinks quebrados.**
     - Skill X cita skill Y que não existe ou mudou de nome → drift.

   - **F. Convenções defasadas.**
     - Agent fala "branch master" mas projeto usa "main" → drift (já corrigimos esse, exemplo histórico).

2. **Apresentar relatório de drift.** Para cada drift identificado, mostrar:

   ```markdown
   ### Drift #N: <título curto>

   **Categoria:** <A-F>
   **Arquivo:** `.claude/agents/research.md`
   **Onde:** linha X, seção "Stability-critical areas"
   **Atual:**
   ```
   <trecho atual>
   ```
   **Proposto:**
   ```
   <trecho novo>
   ```
   **Rationale:** <por que mudar — citar commit ou doc que justifica>
   ```

3. **Confirmar uma por uma.** Para cada drift, perguntar: "Aplicar essa mudança? (sim / não / editar)". 
   - **sim** → Edit.
   - **não** → registrar como "pendente, rejeitado" no relatório.
   - **editar** → permitir ao usuário ajustar o texto proposto antes de aplicar.

4. **Detectar skill faltante.** Se identificou Categoria D, **propor mas não criar**. Mostrar:
   - Sinal observado (3+ ocorrências do mesmo workflow).
   - Nome sugerido (kebab-case).
   - Descrição-rascunho.
   - Recomendar: "Quer brainstormar essa skill em sessão separada? Posso preparar um spec rascunho."

5. **Atualizar meta-info.** Para cada arquivo modificado, se houver campo `last-updated` no frontmatter, atualizar para hoje. Se não houver, não inventar.

6. **Relatório final.**

   ```markdown
   ## Tooling Health Report

   **Data:** <YYYY-MM-DD>
   **Janela analisada:** <período>
   **Drifts identificados:** N
   **Aplicados:** M
   **Rejeitados:** K (com motivo)
   **Skill candidatas propostas:** <lista>

   **Próximos passos sugeridos:**
   - <ações concretas>
   ```

## Integração com `verify-and-document`

O agent `verify-and-document` ganha um passo final: se o diff revelou novos triggers, novos paths sensíveis, ou padrões repetidos, recomendar invocar `evoluir-tooling`. Esse ajuste no agent é parte da Task 7 deste plano.

## Hard rules

- **Cada alteração em `.claude/` é stability-critical.** Confirmar com humano antes de aplicar.
- **Nunca reescrever um agent ou skill inteiro automaticamente.** Só edits cirúrgicos (≤ 30 linhas alteradas por arquivo por rodada).
- **Skills novas não são criadas por esta skill.** Apenas propostas. Criação passa por brainstorming/plan formal.
- **Preservar frontmatter e estrutura.** Só alterar o que justifica o drift.
- **Não tocar `superpowers/` skills globais.** Esta skill cuida apenas de `.claude/` deste projeto.
- **Drift duvidoso** (não tem certeza se é drift ou intencional): registrar no relatório com flag, não aplicar.

## Output (relatório, conforme item 6)
```

- [x] **Step 2: Commit**

```bash
git add .claude/skills/evoluir-tooling.md
git commit -m "feat(skills): adicionar skill evoluir-tooling para manter .claude vivo"
```

---

## Task 6: Atualizar `docs/INDEX.md` para listar skills

**Files:**
- Modify: `docs/INDEX.md`

- [x] **Step 1: Adicionar referência a `.claude/skills/`**

Ler `docs/INDEX.md` atual e adicionar uma linha à seção "Convenções" ou criar uma nova seção curta. Especificamente, inserir após a tabela de estrutura:

```markdown
### Tooling auxiliar

Além de `docs/`, o projeto tem `.claude/` com agents, commands e skills que automatizam workflows recorrentes. Em particular:

- `.claude/agents/` — subagentes (research, plan, implement, verify-and-document).
- `.claude/commands/` — slash commands (`/research`, `/plan`, `/implement`, `/verify-and-doc`).
- `.claude/skills/` — skills project-level: `documentar-conceito`, `documentar-processo`, `atualizar-glossario`, `revisar-marcadores-todo`, `evoluir-tooling`.

Para entender quando cada um se aciona, ver [`dev/AI-WORKFLOW.md`](./dev/AI-WORKFLOW.md).
```

Usar Edit para inserir esse bloco **logo após a seção "## Estrutura"** (antes de "## Por onde começar").

- [x] **Step 2: Commit**

```bash
git add docs/INDEX.md
git commit -m "docs: referenciar .claude/skills no INDEX da documentacao"
```

---

## Task 7: Ajustar `.claude/agents/verify-and-document.md` para recomendar `evoluir-tooling`

⚠️ **STABILITY-CRITICAL** — mudança em agent. Pausar antes de aplicar.

**Files:**
- Modify: `.claude/agents/verify-and-document.md`

- [x] **Step 1: Localizar a seção "Sanity checks" e adicionar passo final**

Ler `.claude/agents/verify-and-document.md`. Localizar a seção `## Output format` ou logo antes dela.

Inserir uma nova seção entre `## Sanity checks (informational only — don't block)` e `## Output format`:

```markdown
## Tooling drift check

Após verificar slices e sanidade do diff, avaliar se as mudanças sugerem necessidade de atualizar `.claude/`:

- Novo conceito ou processo em `docs/dominio/` sem trigger correspondente em `agents/research.md` ou `llm/MAP.md`?
- Novo path sensível introduzido que deveria entrar na lista de stability-critical?
- Renomeação ou movimento de arquivo que torna referências em `.claude/` obsoletas?
- Padrão de pergunta/workflow repetido nas últimas conversas que poderia virar skill?

Se qualquer dos itens acima for verdade, **incluir na seção "Recommendations" do output** uma linha:

> 🔧 Tooling drift detectado: recomendo invocar a skill `evoluir-tooling` para revisar `.claude/`.

Esta skill **não modifica** `.claude/` — só recomenda. A modificação real é responsabilidade da skill `evoluir-tooling` com confirmação humana.
```

- [x] **Step 2: Verificar que a nova seção respeita a estrutura do agent**

Ler o arquivo modificado. Confirmar:
- Seção nova entre "Sanity checks" e "Output format".
- Indentação consistente.
- Sem quebra das hard rules existentes.

- [x] **Step 3: Commit**

```bash
git add .claude/agents/verify-and-document.md
git commit -m "chore(claude): verify-and-document agora recomenda evoluir-tooling em drift"
```

---

## Task 8: Atualizar `docs/dev/AI-WORKFLOW.md` mencionando skills

**Files:**
- Modify: `docs/dev/AI-WORKFLOW.md`

- [x] **Step 1: Adicionar seção de skills no tutorial**

Ler `docs/dev/AI-WORKFLOW.md`. Localizar a seção que descreve os 4 pilares (`MAP.md`, `GLOSSARY.md`, `slices/`, `.claude/`).

Inserir após o pilar "4. `.claude/`" uma nova subseção:

```markdown
### 5. `.claude/skills/` (project-level)

Workflows codificados que o agente invoca automaticamente quando o contexto bate. Diferente de slash commands (você aciona), skills são procedimentos que o agente segue quando reconhece a situação. Skills atuais:

| Skill | Para quem | Quando aciona |
|---|---|---|
| `documentar-conceito` | Professor | Quer registrar conceito novo em `docs/dominio/conceitos/` |
| `documentar-processo` | Professor | Quer registrar processo de trabalho |
| `atualizar-glossario` | Ambos | Termo novo precisa entrar nos dois glossários atomicamente |
| `revisar-marcadores-todo` | Professor | Quer percorrer e resolver `<!-- precisa de revisão do professor -->` |
| `evoluir-tooling` | Dev | Detectar drift entre `.claude/` e a realidade do projeto |

Para criar skills novas, brainstorm + plan formal — não criar ad-hoc.
```

Adicionar também ao final de "Receitas comuns" uma nova receita:

```markdown
### "Quero documentar conceito ou processo novo"

```
# professor diz: "vamos documentar o conceito de inferência leitora"
# agente reconhece e invoca:
documentar-conceito
# segue perguntas guiadas em PT-BR
# ao final, sugere atualizar-glossario se houver termo novo
# e /verify-and-doc para criar slice LLM
```
```

- [x] **Step 2: Commit**

```bash
git add docs/dev/AI-WORKFLOW.md
git commit -m "docs(dev): documentar skills no AI-WORKFLOW"
```

---

## Task 9: Verificação cruzada

**Files:**
- Read-only verification across: `.claude/skills/`, `.claude/agents/verify-and-document.md`, `docs/INDEX.md`, `docs/dev/AI-WORKFLOW.md`

- [ ] **Step 1: Validar frontmatter das 5 skills**

```bash
for f in .claude/skills/*.md; do
  echo "=== $f ==="
  head -5 "$f"
done
```

Esperado: cada arquivo começa com `---`, tem `name:` e `description:`, fecha com `---`. Sem frontmatter quebrado.

- [ ] **Step 2: Validar que as skills se referenciam corretamente**

```bash
grep -l "documentar-conceito\|documentar-processo\|atualizar-glossario\|revisar-marcadores-todo\|evoluir-tooling" .claude/skills/*.md
```

Esperado: skills se citam quando faz sentido (ex.: `documentar-conceito` cita `atualizar-glossario`).

- [ ] **Step 3: Validar que o agent verify-and-document cita evoluir-tooling**

```bash
grep "evoluir-tooling" .claude/agents/verify-and-document.md
```

Esperado: pelo menos uma menção.

- [ ] **Step 4: Validar que INDEX e AI-WORKFLOW listam as 5 skills**

```bash
grep -c "documentar-conceito\|documentar-processo\|atualizar-glossario\|revisar-marcadores-todo\|evoluir-tooling" docs/INDEX.md docs/dev/AI-WORKFLOW.md
```

Esperado: ≥5 em cada arquivo.

- [ ] **Step 5: Commit (apenas se houve correções)**

Se algum passo exigiu correção:

```bash
git add .claude/ docs/
git commit -m "docs: corrigir referencias cruzadas das skills"
```

---

## Self-Review do Plan

**Spec coverage:**

| Critério da spec | Task que cobre |
|---|---|
| 5 skills em `.claude/skills/` | Tasks 1–5 |
| Atomicidade dos dois glossários | Task 3 (hard rule + workflow item 9) |
| Tom PT-BR para skills do professor | Tasks 1, 2, 4 (instruções no body) |
| `evoluir-tooling` em parceria com verify-and-document | Tasks 5 + 7 |
| Skills referenciam-se entre si | Tasks 1, 2 (citam `atualizar-glossario`) |
| INDEX e AI-WORKFLOW atualizados | Tasks 6, 8 |
| Verificação cruzada | Task 9 |

**Placeholder scan:** nenhum `TBD`, `TODO`, "implementar depois". Todo conteúdo de skill está escrito verbatim no plano.

**Type consistency:** nomes de skills (`documentar-conceito`, etc.) idênticos em todas as menções (descrições, citações cruzadas, commits, INDEX, AI-WORKFLOW).

**Ordem:** 5 skills (Tasks 1–5) podem ser executadas em qualquer ordem (independentes). Tasks 6, 8 dependem das 5 existirem. Task 7 é independente, mas marcada ⚠️. Task 9 por último.
