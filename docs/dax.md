# Medidas DAX – Análise de Percepção Afetiva

Este documento apresenta as **colunas calculadas e medidas DAX efetivamente utilizadas** nos dashboards desenvolvidos a partir dos dados da pesquisa, com foco na **mensuração da percepção afetiva dos usuários** e na comparação entre **cenários cromáticos**.

---

## Coluna Calculada – Grau
Relaciona a tabela fato à dimensão de escala de resposta afetiva, trazendo o valor numérico utilizado nas análises.

```DAX
Grau =
RELATED('RespostaAfeto'[Grau])
```

---

## Coluna Calculada – Faixa Etária
Classifica os participantes em faixas etárias padronizadas, permitindo segmentações consistentes nos dashboards.

```DAX
FaixaEtaria =
SWITCH(
    TRUE(),
    [Idade] >= 18 && [Idade] <= 28, "18–28",
    [Idade] >= 29 && [Idade] <= 39, "29–39",
    [Idade] >= 40 && [Idade] <= 50, "40–50",
    [Idade] >= 51 && [Idade] <= 61, "51–61",
    [Idade] >= 62 && [Idade] <= 70, "62–70",
    "Fora da faixa"
)
```

---

## Intensidade Média
Calcula a média geral da intensidade afetiva das respostas, considerando todos os registros válidos.

```DAX
Intensidade Média =
AVERAGE('Medição do afeto'[Grau])
```

---

## Média de Afetos Negativos
Calcula a média das respostas associadas aos adjetivos classificados como **afeto negativo**.

```DAX
Média Negativos =
CALCULATE(
    AVERAGE('Medição do afeto'[Grau]),
    'PerguntasAfeto'[Tipo] = "Negativo"
)
```

---

## Média de Afetos Positivos
Calcula a média das respostas associadas aos adjetivos classificados como **afeto positivo**.

```DAX
Média Positivos =
CALCULATE(
    AVERAGE('Medição do afeto'[Grau]),
    'PerguntasAfeto'[Tipo] = "Positivo"
)
```

---

## Considerações Finais
As expressões DAX apresentadas refletem **exclusivamente as métricas aplicadas nos dashboards**, garantindo transparência, fidelidade metodológica e aderência ao modelo de dados utilizado.

O conjunto demonstra uma aplicação objetiva de DAX para transformar **dados subjetivos de percepção afetiva** em **indicadores claros e comparáveis**, alinhados às boas práticas de **Análise de Dados e Business Intelligence**.
