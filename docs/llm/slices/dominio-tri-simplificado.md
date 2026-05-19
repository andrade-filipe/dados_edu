---
id: dominio-tri-simplificado
title: TRI Simplificada do Projeto
when-to-load: ["TRI", "penalização", "chute", "ponderação", "nota ponderada", "SAEPE", "fórmula", "3 parâmetros", "discriminação", "dificuldade", "acerto ao acaso"]
owners-paths: ["docs/dominio/conceitos/teoria-resposta-item-tri.md", "docs/dominio/formulas/tri-simplificado.md", "docs/dominio/conceitos/escala-saepe.md"]
last-verified: 2026-05-18
version: 2
---

## Resumo

- Versão simplificada da TRI, não a oficial do ENEM.
- TRI oficial usa modelo logístico de 3 parâmetros (a/b/c) estimados por calibração estatística.
- Nossa versão simplificada combina TCT + penalização de chute + ponderação por dificuldade.
- Saída final convertida para escala SAEPE (0-500).

## Pipeline da simplificada

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

## Modelo oficial vs simplificado (resumo)

| Parâmetro | Oficial (INEP) | Nosso |
|---|---|---|
| a (discriminação) | calibração | não estimado |
| b (dificuldade) | calibração | aproximado via TCT (% erro) |
| c (acerto ao acaso) | calibração | heurística 1/3 |

## Fatos-chave

- Pesos da simplificada: fácil = 1, médio = 1,5, difícil = 2.
- Hardcoded `13` (total fáceis na prova de referência) → vira parâmetro quando virar sistema.
- Comparabilidade entre provas só com a TRI oficial (calibrada).

## Cuidados

- ⚠️ Stability-critical: mudar fórmula altera análise de centenas de alunos.
- Saída SAEPE é aproximação; comunicar isso em relatórios.

## Para aprofundar

- [`docs/dominio/formulas/tri-simplificado.md`](../../dominio/formulas/tri-simplificado.md) — fórmulas completas.
- [`docs/dominio/conceitos/teoria-resposta-item-tri.md`](../../dominio/conceitos/teoria-resposta-item-tri.md) — teoria + tabela oficial vs simplificado.
- [`docs/dominio/conceitos/escala-saepe.md`](../../dominio/conceitos/escala-saepe.md) — saída final.
- [`docs/dominio/REFERENCIAS.md`](../../dominio/REFERENCIAS.md#nota-técnica-sobre-tri-no-enem-inep-2011) — Nota Técnica TRI ENEM (INEP).
