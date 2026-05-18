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
