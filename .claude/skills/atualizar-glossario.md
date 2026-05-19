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
