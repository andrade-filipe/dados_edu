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
