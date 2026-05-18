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
