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
