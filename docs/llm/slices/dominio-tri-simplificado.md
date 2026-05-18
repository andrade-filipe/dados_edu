---
id: dominio-tri-simplificado
title: TRI Simplificada do Projeto
when-to-load: ["TRI", "penalização", "chute", "ponderação", "nota ponderada", "SAEPE", "fórmula"]
owners-paths: ["docs/dominio/conceitos/teoria-resposta-item-tri.md", "docs/dominio/formulas/tri-simplificado.md", "docs/dominio/conceitos/escala-saepe.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- Versão simplificada da TRI, não a oficial do ENEM.
- Combina TCT + penalização de chute + ponderação por dificuldade.
- Saída final convertida para escala SAEPE (0-500).

## Pipeline

```
Acertos brutos por dificuldade (F/M/D)
   ↓ penalização: (total - erros) - erros/3
Pontos corrigidos AT, AU, AV
   ↓ ajuste especial das fáceis
AW (se acerto em fáceis < 50%, suaviza AV)
   ↓ ponderação: AT*1 + AU*1,5 + AW*2
AX (nota real ponderada)
   ↓ conversão linear
AZ (0-500)
```

## Fatos-chave

- Pesos: fácil = 1, médio = 1,5, difícil = 2.
- Penalização assume probabilidade média de chute ~1/3 (4 alternativas).
- Hardcoded `13` (total de fáceis na prova de referência) → vira parâmetro quando virar sistema.

## Cuidados

- ⚠️ Stability-critical: mudar fórmula altera análise de centenas de alunos.
- Pesos foram calibrados empiricamente — não trocar sem revisão pedagógica.
- Saída SAEPE é aproximação; comunicar isso em relatórios.

## Para aprofundar

- [`docs/dominio/formulas/tri-simplificado.md`](../../dominio/formulas/tri-simplificado.md) — fórmulas completas com mapeamento de colunas.
- [`docs/dominio/conceitos/teoria-resposta-item-tri.md`](../../dominio/conceitos/teoria-resposta-item-tri.md) — teoria.
- [`docs/dominio/conceitos/escala-saepe.md`](../../dominio/conceitos/escala-saepe.md) — saída final.
