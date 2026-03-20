# Tabela: Movimento

### Esquema
- [Movimento](articles/esquemas/esquemas-movimento.html)

- --

## Exemplo 1: Relatório de Vendas

### Descrição

Retorna as informações e movimentações de venda, sem abater devoluções.

### Observações

Sempre utilize o campo Movimento.Preco_Final_Somado para exibir o total da venda em relatórios que não exibem produtos.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```SQL
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
```
### SQL

```sql
SELECT
CONVERT(VARCHAR(10),Movimento.Data,103) AS Data_Criação,
CONVERT(VARCHAR(10),Movimento.Data_Passou_Efetivacao_Estoque,103) AS Data_Efetivação,
Movimento.Sequencia,
Movimento.Qtde_Total_Prod AS Qtde_Produtos,
Movimento.Preco_Final_Somado AS Total
FROM Movimento
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
WHERE
Movimento.Apagado = 0
AND Movimento.Situacao_Expedicao <> 'A'
AND Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
AND Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
AND Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
```
- --

## Exemplo 2: Relatório de Vendas com Devolução   

### Descrição

Retorna as informações e movimentações de venda, abatendo o valor de devoluções. 

### Observações

São adicionadas as operações 'DEV' e 'CVE' na busca da consulta. As movimentações 'DEV' e 'CVE' terão valores negativos.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```sql
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
```

### SQL

```sql
SELECT
CONVERT(VARCHAR(10),Movimento.Data_Passou_Efetivacao_Estoque,103) AS Data_Efetivação,
Movimento.Sequencia,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Desconto_Total_Geral ELSE -Movimento.Desconto_Total_Geral END AS Desconto_Total,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Frete_Valor_Somado ELSE -Movimento.Frete_Valor_Somado END AS Frete_Total_,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Preco_Final_Somado ELSE -Movimento.Preco_Final_Somado END AS Total
FROM Movimento
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
WHERE
Movimento.Apagado = 0
AND Movimento.Situacao_Expedicao <> 'A'
AND Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
AND Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
AND Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF', 'DEV', 'CVE')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
```
- --

## Exemplo 3: Relatório de Vendas com Cliente

### Observações

Inclui os dados do cliente da movimentação.

Clientes inativos são ignorados.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```sql
INNER JOIN Cli_For ON Movimento.Ordem_Cli_For = Cli_For.Ordem
```

### SQL

```sql
SELECT
Cli_For.Codigo AS Codigo_Cliente,
Cli_For.Nome AS Nome_Cliente,
CONVERT(VARCHAR(10),Movimento.Data_Passou_Efetivacao_Estoque,103) AS Data_Efetivação,
Movimento.Sequencia,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Preco_Final_Somado ELSE -Movimento.Preco_Final_Somado END AS Total
FROM Movimento
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
INNER JOIN Cli_For ON Movimento.Ordem_Cli_For = Cli_For.Ordem
WHERE
Movimento.Apagado = 0
AND Movimento.Situacao_Expedicao <> 'A'
AND Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
AND Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
AND Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF', 'DEV', 'CVE')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
AND Cli_For.Inativo = 0
```
- --

## Exemplo 4: Relatório de Vendas com Vendedor

### Descrição

Inclui os dados do vendedor da movimentação.

### Observações

Por conta do tamanho do campo, é preferível utilizar Funcionarios.Apelido ao invés de Funcionarios.Nome.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```sql
INNER JOIN Funcionarios ON Movimento.Ordem_Vendedor1 = Funcionarios.Ordem
```

### SQL

```sql
SELECT
Funcionarios.Codigo  AS Codigo_Funcionário,
Funcionarios.Apelido  AS Nome_Funcionário,
CONVERT(VARCHAR(10),Movimento.Data_Passou_Efetivacao_Estoque,103) AS Data_Efetivação,
Movimento.Sequencia,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Preco_Final_Somado ELSE -Movimento.Preco_Final_Somado END AS Total
FROM Movimento
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
INNER JOIN Funcionarios ON Movimento.Ordem_Vendedor1 = Funcionarios.Ordem
WHERE
Movimento.Apagado = 0
AND Movimento.Situacao_Expedicao <> 'A'
AND Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
AND Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
AND Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF', 'DEV', 'CVE')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
```
- --


## Exemplo: Relatório de Vendas por Tipo e Recebimento

## Exemplo: Relatório de Vendas por Área

## Exemplo: Relatório de Comissões de Venda

## Exemplo: Relatório de Acompanhamento de Vendas

## Exemplo: Relatório de Lucratividade de Vendas

## Exemplo: Relatório de Vendas com Desconto

## Exemplo: Relatório de Vendas com Frete Próprio

## Exemplo: Relatório de Vendas por Grupo de Operações

## Exemplo: Relatório de Vendas por Grupo de Filiais

## Exemplo: Relatório Comparativo de Vendas e Compras

## Exemplo: Relatório Comparativo Anual de Vendas

## Exemplo: Relatório de Vendas com Nota Fiscal

## Exemplo: Relatório de Vendas por CFOP

## Exemplo: Relatório de Vendas com Liberação Remota

## Exemplo: Relatório de Meta de Vendas

