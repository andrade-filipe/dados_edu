# Skills do Projeto — Design

> Spec gerado por brainstorming · 2026-05-18 · Autor: Filipe Andrade (com Claude)

## Contexto

A fundação de documentação foi entregue (commits `f6e46e9` até `3bf0766`), mas a documentação está **embrionária**: muitos `<!-- precisa de revisão do professor -->`, glossários em dois lugares que podem dessincronizar, conceitos pedagógicos que só o prof. Alikaeli pode preencher, e um tooling (`.claude/agents/`, `.claude/commands/`) que precisará evoluir junto com o projeto.

Para reduzir atrito recorrente nessas frentes, criamos **5 skills** project-level em `.claude/skills/` que codificam os workflows mais frequentes — quatro voltadas ao professor + dev colaborando em domínio, uma meta voltada à manutenção do próprio tooling.

## Goal

Codificar como workflows reutilizáveis (skills) os procedimentos que hoje seriam feitos ad-hoc:
1. Capturar conhecimento pedagógico do professor sem exigir que ele aprenda markdown.
2. Manter os dois glossários atomicamente sincronizados.
3. Atacar sistematicamente os marcadores TODO espalhados pela documentação.
4. Manter o tooling (`.claude/`) atualizado conforme o projeto evolui.

## Non-goals

- Não criar skills para tarefas que ainda não recorrem (importação de planilha, geração de relatório — esperam o projeto amadurecer).
- Não duplicar o que `/verify-and-doc` já faz para `docs/llm/` (a skill 5 é o equivalente para `.claude/`).
- Não substituir as decisões pedagógicas do professor — skills só estruturam o que ele já disse.
- Não traduzir o domínio para EN — skills voltadas ao professor são PT-BR puro.

## Localização e formato

```
.claude/
├── agents/          (já existe)
├── commands/        (já existe)
└── skills/          ← CRIAR
    ├── documentar-conceito.md
    ├── documentar-processo.md
    ├── atualizar-glossario.md
    ├── revisar-marcadores-todo.md
    └── evoluir-tooling.md
```

Cada skill é um único arquivo `.md` com frontmatter:

```markdown
---
name: <kebab-case>
description: <quando usar — 1-2 frases, palavras-chave de trigger>
---

# <Nome legível>

<corpo>
```

## Design de cada skill

### 1. `documentar-conceito`

**Audiência:** professor (primária), dev (secundária).

**Quando ativar:** o usuário quer documentar conceito pedagógico/psicométrico novo. Triggers: "documentar conceito", "escrever sobre [X]", "criar arquivo em conceitos", "preencher template novo-conceito", ou quando o prof explica um conceito e pede "transforma isso em doc".

**Workflow:**
1. Ler `docs/dominio/_templates/novo-conceito.md` para conhecer a estrutura exata.
2. Ler 1-2 arquivos existentes em `docs/dominio/conceitos/` como referência de tom e profundidade.
3. Confirmar o nome do conceito → derivar o slug em kebab-case.
4. Verificar que o arquivo não existe ainda (se existir, sugerir `revisar-marcadores-todo` ou edição direta).
5. **Perguntar uma seção por vez em PT-BR**: "O que é?" → "Por que importa para o nosso projeto?" → ... seguindo a ordem do template.
6. Para cada resposta do prof, preservar a voz dele — não reescrever, no máximo corrigir gramática/pontuação visível.
7. Após coletar tudo, montar o markdown final e mostrar para aprovação antes de gravar.
8. Gravar em `docs/dominio/conceitos/<slug>.md`.
9. Detectar termos do domínio que apareceram na escrita e ainda não estão no glossário → sugerir invocar `atualizar-glossario`.
10. Sugerir rodar `/verify-and-doc` depois para criar a slice LLM espelho.

**Hard rules:**
- Nunca inventar conteúdo pedagógico. Se o prof não souber preencher uma seção, marcar com `<!-- precisa de revisão do professor -->`.
- Nunca pular seções do template "para acelerar".
- Áreas com cor de stability-critical (referência a TRI/TCT/SAEPE) → pedir confirmação adicional antes de gravar.

### 2. `documentar-processo`

**Audiência:** professor (primária).

**Quando ativar:** usuário quer documentar um processo de trabalho (como faz hoje, em que ferramenta, com que artefato). Triggers: "documentar processo", "escrever sobre como faço [X]", "criar arquivo em processos".

**Workflow:** análogo à skill 1, usando `_templates/novo-processo.md`. Diferenças:

- Foca em **passos numerados** ("1. faço X com a planilha; 2. exporto Y; ...") — pergunta "qual o passo 1?", "e depois?", até o prof dizer "acabou".
- Insiste nas seções **"Pontos de dor"** e **"O que seria ideal"** — essas são o insumo do design técnico futuro. Se o prof responder seco, perguntar "o que mais incomoda nesse passo?" / "se você pudesse apertar um botão e o resto saísse pronto, o que esse botão faria?".
- Saída em `docs/dominio/processos/<slug>.md`.

**Hard rules:**
- Mesmas da skill 1.
- "O que seria ideal" não pode ficar vazio — se o prof não souber, gravar `<!-- precisa de revisão do professor -->` com pergunta específica para depois.

### 3. `atualizar-glossario`

**Audiência:** ambos. Também invocável pelo agente `verify-and-document` quando detectar termo novo num diff de domínio.

**Quando ativar:** novo termo PT-BR de domínio precisa entrar no glossário. Triggers: "adicionar ao glossário", "registrar termo", "novo termo do projeto".

**Workflow:**
1. Ler `docs/dominio/GLOSSARIO.md` e `docs/llm/GLOSSARY.md`.
2. Pedir o termo. Normalizar caixa/acentuação para checagem.
3. Verificar duplicata:
   - Se já existe → mostrar entrada atual e perguntar "atualizar?" ou "cancelar?".
   - Se não existe → seguir.
4. Pedir:
   - **Definição** (uma frase clara).
   - **Exemplo de uso** (uma frase em contexto).
   - **Referência** (opcional — BNCC, INEP, etc.).
5. Inserir no `dominio/GLOSSARIO.md`:
   - Posição: ordem alfabética (encontrar termo anterior e posterior).
   - Formato completo conforme convenção do arquivo (Definição/Exemplo/Referência com separador `---`).
6. Inserir no `llm/GLOSSARY.md`:
   - Mesma posição alfabética.
   - Formato compacto: `| <Termo> | <Definição compacta (uma linha)> |`.
7. Mostrar diff dos dois arquivos antes de gravar.
8. Sugerir commit dedicado: `docs(dominio): adicionar termo <X> ao glossario`.

**Hard rules:**
- **Atomicidade:** se um glossário falhar de gravar, reverter o outro. Nunca deixar termo só em um dos dois.
- Grafia consistente nos dois arquivos (caso, acento, hífens).
- Definição compacta no `llm/GLOSSARY.md` ≤120 caracteres.

### 4. `revisar-marcadores-todo`

**Audiência:** professor (primária), dev (apoio).

**Quando ativar:** triggers: "revisar todos", "preencher marcadores", "varrer pendências", "limpar TODOs do domínio".

**Workflow:**
1. Rodar busca em `docs/dominio/` por `<!-- precisa de revisão do professor -->`.
2. Listar resultado: `<arquivo>:<linha> · <heading da seção>`.
3. Apresentar total e perguntar se prof quer começar.
4. Para cada marcador, em ordem:
   - Abrir o arquivo na seção.
   - Mostrar heading + 3-5 linhas de contexto antes e depois.
   - Perguntar em PT-BR: "Aqui o que vai? (Se ainda não quiser preencher, pode dizer 'pular'.)"
   - Aplicar Edit removendo o marcador e inserindo a resposta. Se "pular", deixar como está e seguir.
5. Ao final, reportar:
   - `N` marcadores resolvidos.
   - `M` marcadores restantes (pulados).
   - Sugestão de commit consolidando as resoluções.

**Hard rules:**
- Mostrar contexto suficiente para o prof entender a seção sem reler o arquivo inteiro.
- Não bombardear: um marcador por vez. Se prof cansar, salvar progresso e parar.
- Marcadores em arquivos de fórmula → pausa extra (área sensível).

### 5. `evoluir-tooling`

**Audiência:** dev (primária). Pode ser sugerida pelo `verify-and-document` quando detectar drift.

**Quando ativar:** triggers: "melhorar agents", "atualizar skills", "tooling antiquado", "evoluir .claude", ou periodicamente quando padrões do projeto mudam.

**Workflow:**
1. Ler tudo em `.claude/`:
   - `.claude/agents/*.md`
   - `.claude/commands/*.md`
   - `.claude/skills/*.md`
2. Ler commits recentes em `docs/dominio/` e (futuramente) `src/`:
   ```bash
   git log --since="2 weeks ago" --name-only --pretty=format:"%h %s" -- docs/dominio src
   ```
3. Identificar drift por categoria:
   - **Stability-critical desatualizado** — agent diz "pause em X" mas X não existe mais, ou X mudou de path.
   - **Triggers ausentes** — conceito novo em domínio não tem trigger correspondente no agent research.
   - **Paths obsoletos** — referência a arquivo renomeado/movido.
   - **Skill faltante** — workflow que apareceu repetidamente em conversas e poderia virar skill.
   - **Crosslinks quebrados** — skill X referencia skill Y que não existe.
4. Para cada drift, propor um diff específico com rationale:
   ```markdown
   ### Drift: <título>
   **Arquivo:** `.claude/agents/research.md`
   **Linha:** 42
   **Antes:** "Stability-critical: docs/dominio/formulas/, ..."
   **Depois:** "Stability-critical: docs/dominio/formulas/, docs/dominio/calibracao/, ..."
   **Rationale:** novo subdir `calibracao/` introduzido em commit X afeta análise.
   ```
5. Aplicar cada mudança após confirmação do usuário (uma a uma — alteração no tooling tem efeito multiplicador, errar é caro).
6. Atualizar `last-updated` ou equivalente meta-info do arquivo modificado.
7. Output: relatório "tooling health":
   - N drifts identificados.
   - M aplicados.
   - K pendentes (motivo).

**Integração com `verify-and-document`:** o agente `verify-and-document` ganha um passo extra — após verificar slices LLM, checar se mudanças do diff sugerem update em `.claude/` e, se sim, recomendar invocar `evoluir-tooling`.

**Hard rules:**
- Cada alteração em `.claude/` é stability-critical: confirmar com humano por arquivo.
- Nunca reescrever um agent inteiro automaticamente — só edits cirúrgicos.
- Se identificar necessidade de skill nova, **propor** mas não criar sem aprovação humana.
- Preservar frontmatter e formato — só alterar campos que justificam o diff.

## Convenções

- Frontmatter mínimo: `name`, `description`. Pode incluir `version`, `last-updated` se útil.
- Cada skill começa com uma seção "Quando ativar" e termina com "Hard rules".
- Skills voltadas ao professor: tom PT-BR, perguntas curtas, sem jargão de dev.
- Skills voltadas ao dev: PT-BR mas pode usar termos técnicos (`grep`, `frontmatter`, `kebab-case`).

## Critérios de sucesso

- [ ] Cada skill é invocável pelo nome em conversa natural (description faz match).
- [ ] Professor consegue documentar conceito novo do zero usando `documentar-conceito` sem precisar ver markdown.
- [ ] Adicionar termo via `atualizar-glossario` deixa os dois glossários consistentes em formato e posição.
- [ ] `revisar-marcadores-todo` reduz o backlog de TODOs do domínio em uma sessão.
- [ ] `evoluir-tooling` aplicado num diff propõe mudanças concretas e específicas (não genéricas).
- [ ] `verify-and-document` recomenda `evoluir-tooling` quando detecta drift.

## Riscos e mitigações

| Risco | Mitigação |
|---|---|
| Professor cansa de skill que pergunta seção por seção | Skill aceita "pular" e grava marcador para depois. Sessão pode parar a qualquer momento. |
| Glossários dessincronizam mesmo com skill 3 | Skill 3 grava os dois arquivos no mesmo turn; falha em um aborta o outro. |
| `evoluir-tooling` muda demais de uma vez | Confirmação por arquivo, não em lote. |
| Skills viram letra morta (criadas e nunca usadas) | Descrições específicas para triggers naturais; o agente principal vai propô-las quando contexto encaixar. |

## Plano de implementação

Detalhado em [`docs/superpowers/plans/2026-05-18-skills-projeto.md`](../plans/2026-05-18-skills-projeto.md).

Ordem: criar pasta `.claude/skills/` → 5 skills (uma por task) → ajustar `verify-and-document` para recomendar `evoluir-tooling` → verificação cruzada.
