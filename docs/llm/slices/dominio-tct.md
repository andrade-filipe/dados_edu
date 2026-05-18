---
id: dominio-tct
title: TCT (Teoria Clássica dos Testes)
when-to-load: ["TCT", "teoria clássica", "fácil médio difícil", "classificação de questão"]
owners-paths: ["docs/dominio/conceitos/teoria-classica-testes-tct.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

- TCT classifica questões por proporção de acertos da turma.
- Saída: faixa fácil/médio/difícil que alimenta cálculo de nota ponderada.
- Entrada da [TRI simplificada](./dominio-tri-simplificado.md).

## Faixas (regra do projeto)

| Acertos da turma          | Classificação |
|---------------------------|---------------|
| > 70%                     | Fácil         |
| > 50% e ≤ 70%             | Médio         |
| ≤ 50% (erro > 50%)        | Difícil       |

## Fatos-chave

- Classificação depende da turma — não é absoluta.
- TCT por **descritor** é análise comum: % de acerto agrupada por habilidade.
- TCT é entrada da nossa TRI simplificada (não substitui).

## Cuidados

- Não comparar diretamente fácil/médio/difícil entre turmas — a mesma questão muda de classe.
- Não confundir com TRI oficial (ENEM/SAEB usam modelo estatístico estimado, não proporção simples).

## Para aprofundar

- [`docs/dominio/conceitos/teoria-classica-testes-tct.md`](../../dominio/conceitos/teoria-classica-testes-tct.md)
- [`docs/dominio/formulas/tri-simplificado.md`](../../dominio/formulas/tri-simplificado.md)
