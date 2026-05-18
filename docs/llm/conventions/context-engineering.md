# Engenharia de contexto

> Última atualização: 2026-05-18 · Mantenedor: time dados_edu

Princípios para agentes LLM neste repo carregarem **só o necessário, na hora certa**.

## Princípios

1. **Sempre carregue `MAP.md` + `GLOSSARY.md`.** São o índice e o vocabulário comum.
2. **Carregue slices sob demanda** — máximo 3 por research; mais que isso, divida em sub-research.
3. **Cite arquivos com paths exatos.** "Procure no domínio" não basta — diga "docs/dominio/conceitos/tct.md".
4. **Trust the code over the docs.** Se um slice contradiz a fonte, sinalize e proponha `/verify-and-doc`.
5. **PT-BR para termos de domínio.** EN para conceitos técnicos de implementação (greenfield ainda, mas vale para o futuro).

## Como `/research` decide o que carregar

1. Lê `MAP.md` e identifica triggers no tópico do usuário.
2. Lê `GLOSSARY.md` (sempre).
3. Para cada slice com trigger match, lê o slice inteiro.
4. Se nenhum slice der match, opera com MAP + GLOSSARY apenas e sinaliza no output.

## Como `/plan` consome o output de `/research`

1. Lê os slices que `/research` listou em "slices to load if implementing".
2. Lê `conventions/context-engineering.md` (este arquivo) e `conventions/writing-slices.md` se for criar slice novo.
3. Gera plano com paths exatos.
4. Marca tasks stability-critical (fórmulas, definições teóricas, mapeamento de descritores).

## Como `/verify-and-doc` mantém slices vivos

1. Roda diff contra o último commit.
2. Para cada arquivo alterado em `docs/dominio/` ou `src/`, busca slices com `owners-paths` casando.
3. Atualiza slice afetado, refresca `last-verified`, bumpa `version` se houve mudança substantiva.
4. Cria slice novo se diff trouxe área não-coberta.
