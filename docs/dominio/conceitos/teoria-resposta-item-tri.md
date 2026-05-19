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

## Modelo oficial vs nosso modelo simplificado

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: Nota Técnica TRI ENEM (INEP, 2011) - https://download.inep.gov.br/educacao_basica/enem/nota_tecnica/2011/nota_tecnica_tri.pdf · Status: aguarda validação pedagógica. -->

A TRI oficial usada pelo INEP no ENEM e no SAEB é o **modelo logístico de 3 parâmetros**. Cada item da prova é caracterizado por:

| Parâmetro | Símbolo | O que mede |
|---|---|---|
| Discriminação | **a** | Quão bem a questão distingue alunos com habilidades diferentes. Itens com alta discriminação separam claramente quem domina de quem não domina o conteúdo. |
| Dificuldade | **b** | Nível de habilidade necessário para o aluno ter 50% de probabilidade de acerto (descontando o chute). Itens mais difíceis exigem proficiência mais alta. |
| Acerto ao acaso | **c** | Probabilidade de um aluno acertar a questão por chute. Para questões com 5 alternativas, costuma ficar próximo de 0,20. |

Os parâmetros são **estimados estatisticamente** por calibração com dados reais de aplicação prévia. Uma vez calibrados, eles permitem comparar desempenho entre provas diferentes na mesma escala.

### Diferenças em relação à nossa TRI simplificada

| Parâmetro | TRI oficial (INEP/ENEM/SAEB) | TRI simplificada (este projeto) |
|---|---|---|
| Discriminação (a) | Estimada por calibração estatística | Não estimada — assume itens equivalentes dentro da mesma faixa de dificuldade |
| Dificuldade (b) | Estimada por calibração | Aproximada via TCT (% de erro da turma) |
| Acerto ao acaso (c) | Estimado por calibração | Heurística fixa (penalização de 1/3 dos erros) |
| Comparabilidade entre provas | Sim, dentro da mesma escala calibrada | Não — depende da turma e da prova específica |

A versão simplificada captura a intuição central (penalizar chute, ponderar dificuldade) sem precisar do aparato estatístico de calibração — viável de fazer em planilha, mas com escopo de uso limitado à mesma turma/prova.

## Limites e cuidados

- Não substitui a TRI oficial; é uma heurística operacional.
- Os pesos (1, 1.5, 2) e os fatores de penalização foram calibrados empiricamente pelo professor. Mudá-los exige revisão.
- A versão simplificada não estima discriminação de itens — assume que todas as questões da mesma faixa de dificuldade são equivalentes.

## Relação com outros conceitos

- [TCT](./teoria-classica-testes-tct.md) — entrada da TRI simplificada.
- [Escala SAEPE](./escala-saepe.md) — saída final.
- [Penalização de acerto ao acaso](../GLOSSARIO.md#penalização-de-acerto-ao-acaso) — entrada do ajuste.

## Referências

- [Nota Técnica TRI ENEM (INEP, 2011)](../REFERENCIAS.md#nota-técnica-sobre-tri-no-enem-inep-2011) — documento técnico oficial. Modelo 3-parâmetros descrito na tabela acima.
- [SciELO Brasil](../REFERENCIAS.md#scielo-brasil--scientific-electronic-library-online) — artigos acadêmicos sobre TRI em PT-BR.
- Embretson, S.; Reise, S. — *Item Response Theory for Psychologists*. (Referência clássica EN; ver lista completa em [`REFERENCIAS.md`](../REFERENCIAS.md).)
