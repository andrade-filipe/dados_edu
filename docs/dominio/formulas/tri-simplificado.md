# Fórmula: TRI Simplificada (TCT + Penalização + Ponderação)

> Última atualização: 2026-05-18 · Mantenedor: prof. Alikaeli
>
> ⚠️ Área sensível: revisão pedagógica obrigatória antes de mudar qualquer fórmula desta página.

## Para que serve

Calcular a nota de cada aluno de forma que reflita não só a quantidade de acertos, mas também a dificuldade das questões e a probabilidade de chute. Saída final é convertida para a escala SAEPE (0 a 500).

## Visão geral do cálculo

```
Acertos brutos (por dificuldade)
        ↓
Penalização do acerto ao acaso (descontar chute)
        ↓
Ajuste específico das questões fáceis (caso especial)
        ↓
Nota real ponderada (peso por dificuldade)
        ↓
Conversão para escala SAEPE (0-500)
```

## Variáveis (mapeamento de colunas da planilha)

| Coluna | Significado                                                             |
|--------|-------------------------------------------------------------------------|
| `AN`   | Quantidade de erros em questões **fáceis**                              |
| `AO`   | % de erros em questões fáceis                                           |
| `AP`   | Quantidade de erros em questões **médias**                              |
| `AQ`   | % de erros em questões médias                                           |
| `AR`   | Quantidade de erros em questões **difíceis**                            |
| `AS`   | % de erros em questões difíceis                                         |
| `AT`   | Pontos corrigidos das fáceis (após penalização de chute)                |
| `AU`   | Pontos corrigidos das médias                                            |
| `AV`   | Pontos corrigidos das difíceis (antes do ajuste fácil)                  |
| `AW`   | Ajuste dos pontos das difíceis (passada por SE condicional)             |
| `AX`   | Nota real ponderada por dificuldade                                     |
| `AY`   | Nota real convertida em %                                               |
| `AZ`   | Nota transformada na escala SAEPE (0 a 500)                             |
| `AN$1`, `AP$1`, `AR$1` | Totais máximos de questões fáceis/médias/difíceis (cabeçalho) |

## Fórmulas

### Penalização do acerto ao acaso (por categoria)

A intuição é descontar uma fração estimada de chute proporcional ao número de erros — quem errou muito provavelmente também chutou em algumas das acertadas.

```excel
AT3 = ($AN$1 - AN3) - (AN3 / 3)   # fáceis
AU3 = ($AP$1 - AP3) - (AP3 / 3)   # médias
AV3 = ($AR$1 - AR3) - (AR3 / 3)   # difíceis
```

Leitura: "(total de fáceis menos erros nas fáceis) menos um terço dos erros nas fáceis".

### Ajuste especial das fáceis (`AW`)

Para questões com mais de 50% de erro entre as fáceis, aplica-se uma compensação:

```excel
AW3 = SE((13 - AN3) / 13 < 0,5; MÍNIMO(AV3; 0) + MÁXIMO(AV3; 0) * 0,5; AV3)
```

Leitura: "se o aluno acertou menos de 50% das fáceis, suaviza o `AV`; caso contrário, mantém `AV`".

> O `13` aqui é o total de questões fáceis na prova de referência. Esse valor deve ser parametrizado quando virar código — ele mudará a cada prova.

### Nota real ponderada (`AX`)

```excel
AX3 = MÁXIMO(AT3; 0) * 1 + MÁXIMO(AU3; 0) * 1,5 + AW3 * 2
```

Pesos:
- Fácil = 1
- Média = 1,5
- Difícil = 2

### Conversão para escala SAEPE (`AZ`)

<!-- precisa de revisão do professor para incluir a fórmula exata de transformação linear de AY% para a faixa 0-500. -->

## Exemplo numérico

<!-- precisa de revisão do professor: caso concreto de um aluno com valores reais nas colunas AN..AZ. -->

## Por que essa fórmula foi escolhida

Versão simplificada da TRI, calibrada empiricamente, que captura as duas intuições centrais (penalizar chute, ponderar dificuldade) sem precisar de estimação estatística de parâmetros de item — inviável de fazer manualmente em planilha.

## Limites conhecidos

- Fator `/3` na penalização é heurística — assume probabilidade média de chute de 1/3 (típico de prova de múltipla escolha com 4 alternativas viáveis).
- Peso 1 / 1,5 / 2 é calibração empírica; outra escolha poderia produzir resultados melhores em outros contextos.
- O `13` hardcoded é frágil — quando virar sistema, deve ser parâmetro.

## Referências

- Post original do prof. Alikaeli (arquivo `post_alikaeli` na raiz do repo).
- [TCT](../conceitos/teoria-classica-testes-tct.md), [TRI simplificada](../conceitos/teoria-resposta-item-tri.md), [Escala SAEPE](../conceitos/escala-saepe.md).
