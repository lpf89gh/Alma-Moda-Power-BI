# Medidas DAX — Alma Moda

## 1. Faturamento

```DAX
Faturamento =
SUMX(
    fVendas,
    fVendas[Quantidade Vendida] *
    fVendas[Preço Unitário de Venda (R$)]
)
```

Calcula o faturamento total considerando quantidade vendida multiplicada pelo preço unitário.

## 2. Quantidade Vendida

```DAX
Quantidade Vendida =
SUM(fVendas[Quantidade Vendida])
```

Total de itens vendidos.

## 3. Número de Vendas

```DAX
Número de Vendas =
DISTINCTCOUNT(fVendas[Número NF])
```

Quantidade de vendas distintas, utilizando o número da nota fiscal como identificador.

## 4. Ticket Médio

```DAX
Ticket Médio =
DIVIDE(
    [Faturamento],
    [Número de Vendas]
)
```

Calcula o valor médio por venda.

## 5. Preço Médio por Item

```DAX
Preço Médio por Item =
DIVIDE(
    [Faturamento],
    [Quantidade Vendida]
)
```

Calcula o faturamento médio por unidade vendida.

## 6. Faturamento Ano Anterior

```DAX
Faturamento Ano Anterior =
CALCULATE(
    [Faturamento],
    SAMEPERIODLASTYEAR(dData[Data])
)
```

Retorna o faturamento correspondente ao período equivalente do ano anterior.

## 7. Crescimento Faturamento %

```DAX
Crescimento Faturamento % =
DIVIDE(
    [Faturamento] - [Faturamento Ano Anterior],
    [Faturamento Ano Anterior]
)
```

Calcula a variação percentual do faturamento em relação ao período anterior.

## 8. Participação Faturamento %

```DAX
Participação Faturamento % =
DIVIDE(
    [Faturamento],
    CALCULATE(
        [Faturamento],
        ALL(fVendas)
    )
)
```

Calcula a participação do contexto atual no faturamento total.

## 9. Itens por Venda

```DAX
Itens por Venda =
DIVIDE(
    [Quantidade Vendida],
    [Número de Vendas]
)
```

Calcula a quantidade média de itens por venda.

## 10. Ranking Produto Qtde

Medida utilizada no Top 10 de produtos por quantidade vendida.

```DAX
Ranking Produto Qtde =
VAR QtdeAtual = [Quantidade Vendida]
VAR FatAtual = [Faturamento]
RETURN
    1 +
    COUNTROWS(
        FILTER(
            ALLSELECTED(dProdutos[Nome do Produto]),
            CALCULATE([Quantidade Vendida]) > QtdeAtual
                ||
            (
                CALCULATE([Quantidade Vendida]) = QtdeAtual
                &&
                CALCULATE([Faturamento]) > FatAtual
            )
        )
    )
```

A lógica usa a quantidade vendida como critério principal e o faturamento como critério de desempate. Dessa forma, produtos com a mesma quantidade vendida não recebem a mesma posição.

## Formatação recomendada

- Faturamento: moeda (R$)
- Ticket Médio: moeda (R$)
- Preço Médio por Item: moeda (R$)
- Crescimento Faturamento %: percentual
- Participação Faturamento %: percentual
- Quantidade Vendida: número inteiro
- Número de Vendas: número inteiro
- Itens por Venda: número decimal
