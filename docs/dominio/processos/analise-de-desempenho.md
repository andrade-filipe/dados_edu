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
