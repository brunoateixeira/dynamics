# Movimento

## Vendas

### Descrição

Retorna as informações de movimentações de venda.

### Observações

Vendas apagadas e desfeitas (desefetivadas) são ignoradas.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```SQL
Movimento.Ordem_Filial = Filiais.Ordem
```
### SQL

```sql
SELECT
Movimento.Data_Passou_Efetivacao_Estoque,
Movimento.Sequencia,
Movimento.Desconto_Total_Geral,
Movimento.Frete_Valor_Somado,
Movimento.Preco_Final_Somado
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
## Vendas com Devolução   

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
Movimento.Ordem_Filial = Filiais.Ordem
```

### SQL

```sql
SELECT
Movimento.Data_Passou_Efetivacao_Estoque,
Movimento.Sequencia,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Desconto_Total_Geral ELSE -Movimento.Desconto_Total_Geral END AS Desconto_Geral,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Frete_Valor_Somado ELSE -Movimento.Frete_Valor_Somado END AS Frete_Somado,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Preco_Final_Somado ELSE -Movimento.Preco_Final_Somado END AS Valor_Somado
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

## Vendas com Cliente

### Descrição

Retorna as informações e movimentações de venda, abatendo o valor de devoluções e incluindo os dados do cliente da movimentação.

### Observações

Clientes inativos são ignorados.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```sql
Movimento.Ordem_Filial = Filiais.Ordem
Movimento.Ordem_Cli_For = Cli_For.Ordem
```

### SQL

```sql
SELECT
Cli_For.Codigo,
Cli_For.Nome,
Movimento.Data_Passou_Efetivacao_Estoque,
Movimento.Sequencia,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Desconto_Total_Geral ELSE -Movimento.Desconto_Total_Geral END AS Desconto_Geral,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Frete_Valor_Somado ELSE -Movimento.Frete_Valor_Somado END AS Frete_Somado,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Preco_Final_Somado ELSE -Movimento.Preco_Final_Somado END AS Valor_Somado
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

## Vendas com Vendedor

### Descrição

Retorna as informações e movimentações de venda, abatendo o valor de devoluções e incluindo os dados do vendedor da movimentação.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```sql
Movimento.Ordem_Filial = Filiais.Ordem
Movimento.Ordem_Vendedor1 = Funcionarios.Ordem
```

### SQL

```sql
SELECT
Funcionarios.Codigo,
Funcionarios.Apelido,
Movimento.Data_Passou_Efetivacao_Estoque,
Movimento.Sequencia,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Desconto_Total_Geral ELSE -Movimento.Desconto_Total_Geral END AS Desconto_Geral,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Frete_Valor_Somado ELSE -Movimento.Frete_Valor_Somado END AS Frete_Somado,
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento.Preco_Final_Somado ELSE -Movimento.Preco_Final_Somado END AS Valor_Somado
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

## Vendas com Produtos

### Descrição

Retorna as informações e movimentações de venda, abatendo o valor de devoluções e incluindo os dados dos produtos incluidos na movimentação.

### Observações

Quando listado os produtos na consulta, os campos de valores são da tabela Movimento_Prod_Serv e devem ser somados.

Produtos excluidos das movimentações devem ser ignorados.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |

### Ligações
```sql
Movimento.Ordem_Filial = Filiais.Ordem
Movimento.Ordem = Movimento_Prod_Serv.Ordem_Movimento
Movimento_Prod_Serv.Ordem_Prod_Serv = Prod_Serv.Ordem
```

### SQL

```sql
SELECT
SELECT
CASE WHEN @Filial <> 0 THEN (SELECT CONCAT(Codigo, ' - ', Nome) FROM Filiais WHERE Ordem = @Filial) ELSE 'Todas' END [Filial],
CONVERT(VARCHAR(10),@Data_Inicial,103) as Data_Inicial,
CONVERT(VARCHAR(10),@Data_Final,103) as Data_Final,
Prod_Serv.Codigo,
Prod_Serv.Nome,
SUM(CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento_Prod_Serv.Quantidade ELSE -Movimento_Prod_Serv.Quantidade END) AS Quantidade_Produto_Somado,
SUM(CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento_Prod_Serv.Preco_Final_Relatorio ELSE -Movimento_Prod_Serv.Preco_Final_Relatorio END) AS Valor_Produto_Somado
FROM Movimento
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
INNER JOIN Movimento_Prod_Serv ON Movimento.Ordem = Movimento_Prod_Serv.Ordem_Movimento
INNER JOIN Prod_Serv ON Movimento_Prod_Serv.Ordem_Prod_Serv = Prod_Serv.Ordem
WHERE
	Movimento.Apagado = 0
AND Movimento.Situacao_Expedicao <> 'A'
AND Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
AND Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
AND Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF', 'DEV', 'CVE')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
AND Movimento_Prod_Serv.Linha_Excluida = 0
GROUP BY
Prod_Serv.Codigo,
Prod_Serv.Nome
```

## Vendas com Filtros de Classificação de Produtos

### Descrição

Retorna as informações dos produtos vendidos, abatendo o valor de devoluções e sendo possível filtrar os produtos por classificação de classe, subclasse, grupo, família ou fabricante.

### Observações

Os filtros de classificação de produtos devem ter a opção para que, caso não informados, puxem todos os registros. Por isso, adiciona-se 'OR @Filtro IS NULL' ao filtro.

Exemplo: (Prod_Serv.Ordem_Classe = @Classe OR @Classe IS NULL)

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |
| @Classe | INT | Código da Classe |
| @Subclasse | INT | Código da Subclasse |
| @Grupo | INT | Código da Grupo |
| @Família | INT | Código da Família |
| @Fabricante | INT | Código da Fabricante |

### Ligações
```sql
Movimento.Ordem_Filial = Filiais.Ordem
Movimento.Ordem = Movimento_Prod_Serv.Ordem_Movimento
Movimento_Prod_Serv.Ordem_Prod_Serv = Prod_Serv.Ordem
```

### SQL

```sql
SELECT
Prod_Serv.Codigo,
Prod_Serv.Nome,
SUM(CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento_Prod_Serv.Quantidade ELSE -Movimento_Prod_Serv.Quantidade END) AS Quantidade_Produto_Somado,
SUM(CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento_Prod_Serv.Preco_Final_Relatorio ELSE -Movimento_Prod_Serv.Preco_Final_Relatorio END) AS Valor_Produto_Somado
FROM Movimento
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
INNER JOIN Movimento_Prod_Serv ON Movimento.Ordem = Movimento_Prod_Serv.Ordem_Movimento
INNER JOIN Prod_Serv ON Movimento_Prod_Serv.Ordem_Prod_Serv = Prod_Serv.Ordem
WHERE
Movimento.Apagado = 0
AND Movimento.Situacao_Expedicao <> 'A'
AND Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
AND Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
AND Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF', 'DEV', 'CVE')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
AND Movimento_Prod_Serv.Linha_Excluida = 0
AND (Prod_Serv.Ordem_Classe = @Classe OR @Classe IS NULL)
AND (Prod_Serv.Ordem_Subclasse = @Subclasse OR @Subclasse IS NULL)
AND (Prod_Serv.Ordem_Grupo = @Grupo OR @Grupo IS NULL)
AND (Prod_Serv.Ordem_Familia = @Família OR @Família IS NULL)
AND (Prod_Serv.Ordem_Fabricante = @Fabricante OR @Fabricante IS NULL)
GROUP BY
Prod_Serv.Codigo,
Prod_Serv.Nome
```





