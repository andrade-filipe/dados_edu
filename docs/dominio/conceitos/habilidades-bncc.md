# Habilidades BNCC

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli + time dados_edu

## O que é

A **Base Nacional Comum Curricular (BNCC)** é o documento normativo brasileiro que define as aprendizagens essenciais que todos os alunos devem desenvolver ao longo da Educação Básica. Foi homologada em 20 de dezembro de 2017 e orienta os currículos de todas as redes de ensino (públicas e privadas) do país.

Cada componente curricular (Língua Portuguesa, Matemática, etc.) é organizado em **habilidades** identificadas por códigos padronizados.

## Por que importa para o nosso projeto

A Matriz de Referência SAEB (usada nas avaliações em larga escala como SAEB/SAEPE) e a BNCC **coexistem** — não substituem uma à outra:

- **Matriz SAEB** descreve o que é **avaliado** em larga escala (foco em leitura/interpretação em LP).
- **BNCC** descreve o que deve ser **ensinado** ao longo do ano (foco em todas as áreas: leitura, escrita, oralidade, análise linguística).

O nosso projeto avalia desempenho pela Matriz SAEB (porque o instrumento é compatível), mas ao planejar [intervenções](./sequencias-didaticas.md) e [agrupar alunos](../processos/agrupamento-de-alunos.md), o professor consulta a BNCC para garantir cobertura curricular completa.

## Como funciona — codificação das habilidades

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: https://basenacionalcomum.mec.gov.br/images/BNCC_20dez_site.pdf · Status: aguarda validação pedagógica. -->

Cada habilidade BNCC tem um código no formato:

```
EF<ANO><DISCIPLINA><NÚMERO>
```

Para Língua Portuguesa do 9º ano:

- `EF` = Ensino Fundamental
- `09` = 9º ano (quando a habilidade é específica de um ano)
- `LP` = Língua Portuguesa
- `01, 02, 03...` = número sequencial

Exemplo: `EF09LP01` = primeira habilidade específica do 9º ano de Língua Portuguesa.

Há também códigos compartilhados:

- `EF89LP...` = habilidade compartilhada entre 8º e 9º anos.
- `EF69LP...` = habilidade do 6º ao 9º ano (todos os anos finais do EF).

## Exemplos de habilidades EF09LP

<!-- EXTRAÍDO de fonte oficial em 2026-05-18 · Fonte: BNCC PDF oficial - https://basenacionalcomum.mec.gov.br/images/BNCC_20dez_site.pdf · Status: aguarda validação pedagógica. -->

Amostra de habilidades específicas do 9º ano (transcrição literal de fonte oficial):

- **EF09LP01** — Analisar o fenômeno da disseminação de notícias falsas nas redes sociais e desenvolver estratégias para reconhecê-las, a partir da verificação/avaliação do veículo, fonte, data e local da publicação, autoria, URL, da análise da formatação, da comparação de diferentes fontes, da consulta a sites de curadoria que atestam a fidedignidade do relato dos fatos e denunciam boatos etc.

- **EF09LP03** — Produzir artigos de opinião, tendo em vista o contexto de produção dado, assumindo posição diante de tema polêmico, argumentando de acordo com a estrutura própria desse tipo de texto e utilizando diferentes tipos de argumentos — de autoridade, comprovação, exemplificação, princípio etc.

- **EF09LP07** — Comparar o uso de regência verbal e regência nominal na norma-padrão com seu uso no português brasileiro coloquial oral.

- **EF09LP08** — Identificar, em textos lidos e em produções próprias, a relação que conjunções (e locuções conjuntivas) coordenativas e subordinativas estabelecem entre as orações que conectam.

A lista completa está no documento oficial da BNCC (link em [`REFERENCIAS.md`](../REFERENCIAS.md#bncc--base-nacional-comum-curricular)).

## Relação com a Matriz SAEB

<!-- precisa de revisão do professor: descrever como você usa BNCC e Matriz SAEB em conjunto na prática (ex.: SAEB orienta o que medimos; BNCC orienta o que ensinamos antes/depois). Exemplo concreto de uma sequência didática que cobre uma habilidade BNCC e desenvolve um descritor SAEB ao mesmo tempo. -->

## Limites e cuidados

- BNCC define competências e habilidades por ano, mas redes de ensino têm liberdade para organizar internamente. Verifique se a rede da escola adota a BNCC pura ou um currículo derivado.
- Nem toda habilidade BNCC casa 1:1 com descritor SAEB — alguns descritores cobrem várias habilidades; algumas habilidades não são avaliáveis por prova escrita.

## Relação com outros conceitos

- [Descritores SAEB](./descritores.md) — referencial de avaliação, complementar à BNCC.
- [Sequências didáticas](./sequencias-didaticas.md) — onde BNCC vira plano de aula.
- [Avaliação diagnóstica](./avaliacao-diagnostica.md) — fecha o loop: identificar lacunas por descritor SAEB e replanejar cobertura por habilidade BNCC.

## Referências

- [BNCC — portal oficial e PDF completo](../REFERENCIAS.md#bncc--base-nacional-comum-curricular).
- [MEC — Ministério da Educação](../REFERENCIAS.md#mec--ministério-da-educação).
