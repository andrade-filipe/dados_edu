# Como escrever slices

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Slices em `docs/llm/slices/` são pedaços compactos de documentação carregados sob demanda pelo agente LLM em `/research` e `/plan`. Otimizados para **densidade**, não para leitura humana confortável.

## Regras de ouro

- **≤200 linhas por slice.** Se passa disso, divida.
- **Frontmatter YAML obrigatório** com:
  - `id` — identificador único em kebab-case.
  - `title` — título legível.
  - `when-to-load` — lista de triggers (palavras/expressões) que indicam que esse slice é relevante.
  - `owners-paths` — globs dos arquivos que esse slice cobre (em `docs/dominio/` ou `src/`).
  - `last-verified` — data ISO do último cross-check com a fonte.
  - `version` — inteiro, incrementa em mudança substantiva.
- **Anchors estáveis** — use cabeçalhos `## <Nome>` que não mudam à toa. Outros docs podem referenciar `slices/foo.md#nome-anchor`.
- **Cite a fonte** — toda afirmação importante referencia um arquivo de domínio ou linha de código.
- **Tom telegráfico.** Bullet points, tabelas, frases curtas. Prose explicativa vai em `docs/dominio/` ou `docs/dev/`.

## Esqueleto

```markdown
---
id: meu-slice
title: Meu Slice
when-to-load: ["palavra1", "expressao 2", "termo 3"]
owners-paths: ["docs/dominio/conceitos/meu-conceito.md"]
last-verified: 2026-05-18
version: 1
---

## Resumo

<2-3 bullets capturando o essencial>

## Fatos-chave

- <fato 1, com referência>
- <fato 2>

## Cuidados

- <armadilha conhecida>

## Para aprofundar

- [`docs/dominio/.../arquivo.md`](../../dominio/...)
```

## O que NÃO colocar em slice

- Onboarding humano (vai em `docs/dominio/README.md` ou `docs/dev/README.md`).
- Exemplos longos e ilustrativos (vão em `docs/dominio/`).
- Tutoriais (vão em `docs/dev/`).

## Quando criar slice novo

- Surgiu uma área que `/research` precisa carregar e que ainda não tem slice.
- Slice existente passou de 200 linhas → divida em dois.
- Novo conceito grande chegou em `docs/dominio/` → crie slice espelho.
