# Teoria de Resposta ao Item (TRI) — versão simplificada

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

A Teoria de Resposta ao Item (TRI) é um modelo psicométrico avançado que estima a habilidade do aluno considerando: a dificuldade de cada questão, o poder de discriminação da questão e a probabilidade de acerto por chute. É o modelo usado oficialmente no ENEM e em avaliações em larga escala como SAEB e SAEPE.

A TRI **completa** exige conhecimento estatístico e ferramentas específicas. No projeto, usamos uma **versão simplificada** que captura a intuição central — penalizar o chute e ponderar a dificuldade — sem o aparato matemático completo.

## Por que importa para o nosso projeto

A TRI simplificada nos permite gerar uma nota que reflete mais o conhecimento real do aluno, e não só "quantas questões ele acertou". Isso é especialmente útil quando comparamos desempenho em provas com mesma matriz mas questões diferentes.

## Como funciona

Versão simplificada usada no projeto, em três etapas:

1. **Classificar questões por dificuldade** via [TCT](./teoria-classica-testes-tct.md).
2. **Penalizar acerto ao acaso** — subtrair uma fração estimada de chute em cada categoria de dificuldade.
3. **Ponderar a nota** dando peso maior a questões mais difíceis.

A fórmula exata está em [`formulas/tri-simplificado.md`](../formulas/tri-simplificado.md).

## Exemplo concreto

Um aluno acerta 8 questões fáceis, 4 médias e 2 difíceis (total 14). Pelo escore bruto seria 14. Com a TRI simplificada, ele recebe uma nota ponderada que reconhece o esforço maior nas difíceis e desconta possível chute nas fáceis. O resultado é convertido para a [escala SAEPE](./escala-saepe.md) (0 a 500).

## Limites e cuidados

- Não substitui a TRI oficial; é uma heurística operacional.
- Os pesos (1, 1.5, 2) e os fatores de penalização foram calibrados empiricamente pelo professor. Mudá-los exige revisão.
- A versão simplificada não estima discriminação de itens — assume que todas as questões da mesma faixa de dificuldade são equivalentes.

## Relação com outros conceitos

- [TCT](./teoria-classica-testes-tct.md) — entrada da TRI simplificada.
- [Escala SAEPE](./escala-saepe.md) — saída final.
- [Penalização de acerto ao acaso](../GLOSSARIO.md#penalização-de-acerto-ao-acaso) — entrada do ajuste.

## Referências

- INEP. *Nota técnica sobre TRI no ENEM*.
- Embretson, S.; Reise, S. *Item Response Theory for Psychologists*.
