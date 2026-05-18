# MAP (LLM Index)

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Índice global. Sempre carregado pelo agente em `/research` e `/plan`. Lista slices disponíveis com triggers.

## Layout do repositório

```
/
├── post_alikaeli              # post original do professor (contexto humano)
├── README.md                  # vitrine + ponteiros
├── docs/
│   ├── INDEX.md               # mapa das pastas
│   ├── dominio/               # FONTE CANÔNICA de domínio (PT-BR)
│   ├── dev/                   # docs para devs
│   ├── llm/                   # este diretório (slices + convenções)
│   └── superpowers/
│       ├── specs/             # designs aprovados
│       └── plans/             # planos executáveis
└── .claude/
    ├── agents/                # research, plan, implement, verify-and-document
    └── commands/              # /research, /plan, /implement, /verify-and-doc
```

## Quando carregar qual slice

| Se o tópico do usuário contém…                         | Carregue                          |
|--------------------------------------------------------|-----------------------------------|
| "avaliação", "diagnóstica", "simulado", "ciclo"        | `slices/dominio-avaliacao-ciclo.md` |
| "descritor", "matriz SAEB", "BNCC", "habilidade"       | `slices/dominio-descritores.md`     |
| "TCT", "teoria clássica", "fácil/médio/difícil"        | `slices/dominio-tct.md`             |
| "TRI", "penalização", "chute", "ponderação", "SAEPE"   | `slices/dominio-tri-simplificado.md` |
| "análise", "agrupamento", "nível", "intervenção"       | `slices/dominio-analise-pedagogica.md` |
| "relatório", "feedback", "comunicação de resultado"    | `slices/dominio-relatorios.md`      |

## Disambiguation traps

- **Dois glossários:** `docs/dominio/GLOSSARIO.md` (completo, humano) vs. `docs/llm/GLOSSARY.md` (compacto, agente). Fonte da verdade é o de domínio.
- **TRI no projeto ≠ TRI oficial:** o que chamamos de TRI é a versão simplificada. Para TRI oficial do ENEM/SAEB, deixar claro.
- **Pré-código:** nenhum arquivo em `src/` ainda. `/research` em tópicos de implementação cai apenas em domínio.

## Convenções

- Veja [`conventions/writing-slices.md`](./conventions/writing-slices.md) e [`conventions/context-engineering.md`](./conventions/context-engineering.md).
