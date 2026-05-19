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
