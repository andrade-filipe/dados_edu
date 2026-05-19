# Escala SAEPE

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli

## O que é

Escala numérica de **0 a 500** usada para reportar proficiência em avaliações em larga escala no Sistema de Avaliação Educacional de Pernambuco (SAEPE). É a mesma família de escala usada em SAEB e Prova Brasil.

## Por que importa para o nosso projeto

A saída final do cálculo de desempenho do aluno (após TCT + TRI simplificada) é convertida para essa escala. Isso permite comparar o desempenho interno com referências oficiais conhecidas pelos professores e gestores.

## Como funciona

Após calcular a nota real ponderada (`AX` na planilha) e convertê-la em percentual (`AY`), aplica-se uma transformação linear que leva o resultado para a faixa 0-500. A fórmula exata e a calibração estão em [`../formulas/tri-simplificado.md`](../formulas/tri-simplificado.md).

## Padrões de Desempenho do SAEPE

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: https://avaliacaoemonitoramentopernambuco.caeddigital.net/ + revistas SAEPE 2018/2021 publicadas pelo CAEd · Status: aguarda validação pedagógica. -->

O SAEPE classifica o desempenho dos alunos em **4 padrões** ao longo da escala 0-500:

| Padrão | O que significa pedagogicamente (descrição geral) |
|---|---|
| **Elementar I** | Aluno em estágio inicial de desenvolvimento das habilidades esperadas para a etapa. Demanda intervenção intensiva. |
| **Elementar II** | Aluno em desenvolvimento, ainda abaixo do esperado para a etapa. Demanda atenção pedagógica focada. |
| **Básico** | Aluno demonstra domínio das habilidades mínimas para a etapa. Pode avançar com apoio direcionado. |
| **Desejável** | Aluno demonstra domínio pleno (ou acima) das habilidades esperadas para a etapa. |

### Pontos de corte por etapa e disciplina

Os pontos de corte exatos variam por etapa (5º EF, 9º EF, 3º EM) e disciplina (Língua Portuguesa, Matemática).

**Para Matemática 9º ano EF (extraído do SAEPE 2018):**

| Padrão | Faixa de pontos |
|---|---|
| Elementar I | até 225 |
| Elementar II | 225 a 245 |
| Básico | 245 a 280 |
| Desejável | acima de 280 |

**Para Língua Portuguesa 9º ano EF:**

<!-- precisa de revisão do professor: consultar o relatório oficial SAEPE 2021 (PDF: https://avaliacaoemonitoramentopernambuco.caeddigital.net/resources/arquivos/colecoes/SAEPE_2021/PE2021RPLP.pdf) e preencher os 4 pontos de corte específicos para LP 9º EF. Pontos de corte variam entre disciplinas — não copiar os de Matemática acima. -->

## Limites e cuidados

- Escala SAEPE oficial é calibrada com TRI completa; nossa conversão é aproximada via TRI simplificada — diferenças são esperadas e devem ser comunicadas em relatórios.

## Relação com outros conceitos

- [TRI simplificada](./teoria-resposta-item-tri.md)
- [TCT](./teoria-classica-testes-tct.md)

## Referências

- [SAEPE — portal oficial (CAEd)](../REFERENCIAS.md#saepe--sistema-de-avaliação-educacional-de-pernambuco) — fonte oficial de calibração, revistas pedagógicas anuais e relatórios técnicos.
- [Escala de Proficiência SAEB (INEP)](../REFERENCIAS.md#saeb--sistema-de-avaliação-da-educação-básica) — escala-mãe da qual o SAEPE deriva os pontos de corte.
