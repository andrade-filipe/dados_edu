# Domínio Pedagógico

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli + time dados_edu

Esta pasta é a **fonte canônica** do conhecimento pedagógico do projeto. Tudo que dizemos sobre avaliação, descritores, TCT, TRI, intervenção pedagógica e escala SAEPE começa aqui.

## Estrutura

| Subpasta       | O que vai aqui                                                                       |
|----------------|--------------------------------------------------------------------------------------|
| [`conceitos/`](./conceitos/)   | Teoria. Um arquivo por conceito (TCT, TRI, descritores, níveis de leitura…).        |
| [`processos/`](./processos/)   | Como o trabalho acontece hoje (ciclo avaliativo, coleta, análise, agrupamento).     |
| [`formulas/`](./formulas/)     | Matemática real do projeto (fórmulas de TRI simplificado, planilhas, exemplos).     |
| [`_templates/`](./_templates/) | Esqueletos para o professor preencher conceitos/processos novos sem precisar criar markdown do zero. |
| [`GLOSSARIO.md`](./GLOSSARIO.md) | Vocabulário PT-BR com definições completas e exemplos de uso.                     |
| [`REFERENCIAS.md`](./REFERENCIAS.md) | Fontes oficiais e confiáveis para aprofundamento (SAEPE, INEP, BNCC, etc.).   |

## Para o professor: como contribuir

Você é a fonte da verdade do conteúdo pedagógico deste projeto. Aqui está como contribuir sem precisar virar dev:

### Adicionar um conceito novo

1. Abra [`_templates/novo-conceito.md`](./_templates/novo-conceito.md) e copie o conteúdo.
2. Crie um arquivo novo em `conceitos/` com nome em minúsculas separado por hífen: `meu-conceito-novo.md`.
3. Cole o template no arquivo e preencha cada seção.
4. Se não souber editar diretamente, peça ao Claude: "preenche esse template com base no que vou te explicar sobre X" e cole sua explicação.

### Adicionar um processo novo

Igual ao conceito, mas usando [`_templates/novo-processo.md`](./_templates/novo-processo.md) e salvando em `processos/`.

### Atualizar uma fórmula

Fórmulas vivem em `formulas/`. Se você descobrir que uma fórmula está errada ou tem uma versão melhor:

1. Abra o arquivo da fórmula.
2. Edite o bloco da fórmula explicando **por que** mudou (essa parte é crítica).
3. Marque o cabeçalho com a data nova.
4. Se possível, peça revisão a outro professor da área antes de confirmar.

> ⚠️ Mexer em fórmula é considerado "área sensível" no nosso workflow. Quem implementar mudança aqui pausa e pede confirmação antes de aplicar — é proteção contra erro silencioso.

### Adicionar termo ao glossário

Abra [`GLOSSARIO.md`](./GLOSSARIO.md) e adicione na ordem alfabética. Cada termo tem três campos:

- **Definição** — uma frase clara.
- **Exemplo de uso** — uma frase em contexto.
- **Referência** (opcional) — de onde a definição vem (BNCC, INEP, Vygotsky, etc.). Veja [`REFERENCIAS.md`](./REFERENCIAS.md) para a lista completa de fontes oficiais com URLs.

## Onde tirar dúvidas

Para fundamentação externa (SAEPE oficial, matriz de descritores SAEB, BNCC, nota técnica TRI do INEP, etc.) veja [`REFERENCIAS.md`](./REFERENCIAS.md). Apenas fontes oficiais ou periódicos acadêmicos com revisão por pares.

## Para o dev: como consumir

Tudo em `dominio/` é texto narrativo, otimizado para humanos. Quando o agente LLM precisa do mesmo conteúdo de forma compacta, ele lê os equivalentes em [`docs/llm/slices/`](../llm/slices/). Não duplique manualmente — rode `/verify-and-doc` para sincronizar.
