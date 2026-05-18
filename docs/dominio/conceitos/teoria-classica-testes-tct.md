# Teoria Clássica dos Testes (TCT)

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

A Teoria Clássica dos Testes (TCT) é um modelo psicométrico que avalia o desempenho dos alunos analisando a proporção de acertos e erros em cada questão de uma avaliação. É o modelo mais antigo e mais simples, base para muitos sistemas de avaliação interna.

## Por que importa para o nosso projeto

A TCT é o ponto de partida da nossa análise. Ela permite, com pouco esforço, classificar as questões da prova em fácil, médio e difícil — e isso direciona como vamos ponderar o desempenho de cada aluno e onde concentrar a intervenção pedagógica.

## Como funciona

A TCT classifica questões pela proporção de acertos da turma:

| Faixa de acertos                | Classificação |
|---------------------------------|---------------|
| Acima de 70%                    | Fácil         |
| Acima de 50% e até 70%          | Médio         |
| 50% ou menos (erro > 50%)       | Difícil       |

Essa classificação alimenta o cálculo da nota ponderada, em que questões difíceis valem mais que fáceis. Veja [`formulas/tri-simplificado.md`](../formulas/tri-simplificado.md).

## Exemplo concreto

Em uma turma de 30 alunos, a questão 1 teve 25 acertos (83%) — classificada como **fácil**. A questão 7 teve 12 acertos (40%) — classificada como **difícil**.

## Limites e cuidados

- A TCT depende da turma: a mesma questão pode ser "fácil" numa turma e "difícil" em outra. Isso impede comparação direta entre turmas.
- A TCT não considera padrão de chute. Por isso, no projeto, combinamos TCT com uma versão simplificada de TRI.

## Relação com outros conceitos

- [Teoria de Resposta ao Item (TRI)](./teoria-resposta-item-tri.md) — modelo mais sofisticado, usado em ENEM/SAEB.
- [Descritores](./descritores.md) — cada questão tem um descritor, e a análise TCT pode ser feita por descritor.

## Referências

- Pasquali, L. *Psicometria: teoria dos testes na psicologia e na educação*.
