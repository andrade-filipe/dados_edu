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
