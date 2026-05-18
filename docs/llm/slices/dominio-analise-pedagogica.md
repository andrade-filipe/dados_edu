---
id: dominio-analise-pedagogica
title: Análise Pedagógica e Agrupamento
when-to-load: ["análise", "agrupamento", "nível", "intervenção", "perfil"]
owners-paths: ["docs/dominio/processos/analise-de-desempenho.md", "docs/dominio/processos/agrupamento-de-alunos.md", "docs/dominio/conceitos/niveis-de-leitura.md", "docs/dominio/conceitos/sequencias-didaticas.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- Análise transforma planilha bruta em insights acionáveis: descritores críticos, perfis de aluno, agrupamentos.
- Agrupamento por nível guia a escolha da sequência didática.

## Pipeline de análise

```
Dados coletados
   ↓
% de acerto por questão (TCT)
% de acerto por descritor
   ↓
Marcar descritores críticos (limiar configurável)
Identificar perfil de cada aluno
   ↓
Agrupar alunos por perfil
   ↓
Associar sequência didática a cada grupo
```

## Fatos-chave

- Hoje feito em planilha + apoio de IA. Trabalhoso.
- Limiar de "crítico" não é fixo — depende do contexto da turma.
- Agrupamento equilibrado (tamanhos parecidos) exige iteração manual.

## Cuidados

- Não rotular o aluno permanentemente pelo nível — comunicação importa.
- Reavaliar agrupamentos a cada ciclo (alunos transitam).

## Para aprofundar

- [`docs/dominio/processos/analise-de-desempenho.md`](../../dominio/processos/analise-de-desempenho.md)
- [`docs/dominio/processos/agrupamento-de-alunos.md`](../../dominio/processos/agrupamento-de-alunos.md)
- [`docs/dominio/conceitos/niveis-de-leitura.md`](../../dominio/conceitos/niveis-de-leitura.md)
