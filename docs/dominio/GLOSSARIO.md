# Glossário (Domínio Pedagógico)

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli + time dados_edu

Vocabulário PT-BR do projeto. Ordem alfabética. Cada termo tem definição, exemplo de uso e (quando aplicável) referência.

---

## Agrupamento por nível

**Definição:** Organização dos alunos em grupos baseada no perfil de desempenho identificado nas avaliações, para aplicar intervenções específicas.

**Exemplo de uso:** "Depois da análise por descritor, fiz o agrupamento por nível e cada grupo recebeu uma sequência didática diferente."

---

## Avaliação diagnóstica

**Definição:** Instrumento aplicado para identificar o nível de conhecimento atual dos alunos em determinadas habilidades, antes de iniciar uma intervenção.

**Exemplo de uso:** "Apliquei a avaliação diagnóstica de leitura na primeira semana para mapear quais descritores precisam de mais trabalho."

---

## Descritor

**Definição:** Habilidade curricular específica que uma questão avalia. Cada descritor é uma referência codificada da BNCC ou da matriz curricular adotada (ex.: D1, D2 da Matriz SAEB).

**Exemplo de uso:** "O descritor D5 mede a capacidade de localizar informação explícita no texto."

**Referência:** Matriz de Referência SAEB / BNCC.

---

## Escala SAEPE

**Definição:** Escala de proficiência de 0 a 500 utilizada para reportar desempenho em avaliações em larga escala em Pernambuco. O sistema do projeto converte notas internas para essa escala como saída final.

**Exemplo de uso:** "O aluno teve 320 na escala SAEPE — está em nível básico para o 9º ano."

**Referência:** Sistema de Avaliação Educacional de Pernambuco.

---

## Gênero textual

**Definição:** Tipo de texto definido por sua função social e estrutura (conto, notícia, artigo de opinião, etc.). A curadoria de gêneros para sequências didáticas considera o currículo e os descritores a desenvolver.

**Exemplo de uso:** "Para trabalhar localização de informação, escolhi gêneros notícia e verbete de enciclopédia."

---

## Intervenção pedagógica

**Definição:** Conjunto de atividades planejadas para superar dificuldades identificadas em uma avaliação. No projeto, cada grupo de alunos recebe intervenção específica.

**Exemplo de uso:** "A intervenção para o grupo de leitor iniciante focou em decodificação antes de avançar para inferência."

---

## Nível de leitura

**Definição:** Classificação do aluno em estágios de proficiência leitora (leitor iniciante, em desenvolvimento, proficiente, etc.). Usado para agrupar alunos em intervenções específicas.

**Exemplo de uso:** "Identificamos três níveis de leitura na turma; cada um recebeu uma sequência didática diferente."

---

## Penalização de acerto ao acaso

**Definição:** Ajuste matemático aplicado ao escore para reduzir a contribuição de questões cujo acerto pode ter sido resultado de chute. Inspirado em TRI mas implementado de forma simplificada via planilha.

**Exemplo de uso:** "Aplico penalização ao acaso nas questões fáceis para que o resultado reflita mais o conhecimento real."

**Referência:** Veja [`formulas/tri-simplificado.md`](./formulas/tri-simplificado.md) para a fórmula exata usada no projeto.

---

## Sequência didática

**Definição:** Conjunto encadeado de atividades planejadas para desenvolver um conjunto específico de habilidades em determinado tempo. No projeto, cada perfil de aluno recebe uma sequência adequada.

**Exemplo de uso:** "Montei uma sequência didática de quatro aulas focada em inferência."

**Referência:** Dolz, Noverraz e Schneuwly (2004).

---

## Simulado

**Definição:** Avaliação aplicada simulando provas externas (ENEM, SAEB, SAEPE). Serve tanto para diagnóstico quanto para treinamento.

**Exemplo de uso:** "O simulado de fim de bimestre testou descritores de leitura e gramática."

---

## TCT (Teoria Clássica dos Testes)

**Definição:** Modelo psicométrico que avalia desempenho por proporção de acerto. No projeto, classifica questões em fácil/médio/difícil:
- erro > 50% → difícil
- 50% < acerto ≤ 70% → médio
- acerto > 70% → fácil

**Exemplo de uso:** "Pela TCT, três questões da prova foram classificadas como difíceis — vão receber atenção na revisão."

**Referência:** Veja [`conceitos/teoria-classica-testes-tct.md`](./conceitos/teoria-classica-testes-tct.md).

---

## TRI (Teoria de Resposta ao Item)

**Definição:** Modelo psicométrico que avalia o desempenho considerando a dificuldade de cada questão e padrões de chute. Usado em ENEM e SAEB. No projeto, aplicamos uma versão simplificada baseada em TCT + penalizações.

**Exemplo de uso:** "A TRI simplificada que uso pondera fáceis, médias e difíceis com pesos diferentes."

**Referência:** Veja [`conceitos/teoria-resposta-item-tri.md`](./conceitos/teoria-resposta-item-tri.md) e [`formulas/tri-simplificado.md`](./formulas/tri-simplificado.md).
