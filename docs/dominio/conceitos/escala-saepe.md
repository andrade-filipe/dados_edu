# Escala SAEPE

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

Escala numérica de **0 a 500** usada para reportar proficiência em avaliações em larga escala no Sistema de Avaliação Educacional de Pernambuco (SAEPE). É a mesma família de escala usada em SAEB e Prova Brasil.

## Por que importa para o nosso projeto

A saída final do cálculo de desempenho do aluno (após TCT + TRI simplificada) é convertida para essa escala. Isso permite comparar o desempenho interno com referências oficiais conhecidas pelos professores e gestores.

## Como funciona

Após calcular a nota real ponderada (`AX` na planilha) e convertê-la em percentual (`AY`), aplica-se uma transformação linear que leva o resultado para a faixa 0-500. A fórmula exata e a calibração estão em [`../formulas/tri-simplificado.md`](../formulas/tri-simplificado.md).

## Faixas de proficiência (referência)

<!-- precisa de revisão do professor para indicar as faixas oficiais SAEPE por etapa (ex.: até X = abaixo do básico; X-Y = básico; etc.) -->

## Limites e cuidados

- Escala SAEPE oficial é calibrada com TRI completa; nossa conversão é aproximada via TRI simplificada — diferenças são esperadas e devem ser comunicadas em relatórios.

## Relação com outros conceitos

- [TRI simplificada](./teoria-resposta-item-tri.md)
- [TCT](./teoria-classica-testes-tct.md)

## Referências

- Secretaria de Educação de Pernambuco — documentos SAEPE.
