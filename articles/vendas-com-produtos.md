# Movimento

## Vendas com Produtos Base

### Descrição

Retorna as informações e movimentações de venda, incluindo os dados dos produtos incluidos na movimentação.

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
Prod_Serv.Codigo AS Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
SUM(Movimento_Prod_Serv.Quantidade) AS Quantidade_Produto_Somado,
SUM(Movimento_Prod_Serv.Preco_Final_Relatorio) AS Valor_Produto_Somado
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
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
AND Movimento_Prod_Serv.Linha_Excluida = 0
GROUP BY
Prod_Serv.Codigo,
Prod_Serv.Nome
ORDER BY 
Valor_Produto_Somado DESC

```

## Vendas com Produtos com Devolução

### Descrição

Retorna as informações e movimentações de venda, abatendo o valor de devoluções e incluindo os dados dos produtos incluidos na movimentação.

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
Movimento.Ordem = Movimento_Prod_Serv.Ordem_Movimento
Movimento_Prod_Serv.Ordem_Prod_Serv = Prod_Serv.Ordem
```

### SQL

```sql
SELECT
Prod_Serv.Codigo AS Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
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
ORDER BY 
Valor_Produto_Somado DESC
```

## Vendas com Produtos com Vendedor

### Descrição

Retorna as informações e movimentações de venda, incluindo os dados dos vendedores na movimentação.

### Observações

Quando listados produtos na consulta, são utilizados os campos Movimento_Prod_Serv.Ordem_Vendedor e/ou Movimento_Prod_Serv.Ordem_Vendedor2 para realizar as ligações de Funcionários.

Por conta do tamanho do campo, é preferível utilizar Funcionarios.Apelido ao invés de Funcionarios.Nome.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Código da Filial |
| @Vendedor | INT | Código do Vendedor |

### Ligações
```sql
Movimento.Ordem_Filial = Filiais.Ordem
Movimento.Ordem = Movimento_Prod_Serv.Ordem_Movimento
Movimento_Prod_Serv.Ordem_Prod_Serv = Prod_Serv.Ordem
Movimento_Prod_Serv.Ordem_Vendedor = Funcionarios.Ordem
```

### SQL

```sql
SELECT
Funcionarios.Codigo AS Codigo_Funcionário,
Funcionarios.Apelido AS Nome_Funcionário,
Prod_Serv.Codigo AS Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
SUM(CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento_Prod_Serv.Quantidade ELSE -Movimento_Prod_Serv.Quantidade END) AS Quantidade_Produto_Somado,
SUM(CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF') THEN Movimento_Prod_Serv.Preco_Final_Relatorio ELSE -Movimento_Prod_Serv.Preco_Final_Relatorio END) AS Valor_Produto_Somado
FROM Movimento
INNER JOIN Filiais ON Movimento.Ordem_Filial = Filiais.Ordem
INNER JOIN Movimento_Prod_Serv ON Movimento.Ordem = Movimento_Prod_Serv.Ordem_Movimento
INNER JOIN Prod_Serv ON Movimento_Prod_Serv.Ordem_Prod_Serv = Prod_Serv.Ordem
INNER JOIN Movimento_Prod_Serv.Ordem_Vendedor = Funcionarios.Ordem
WHERE
Movimento.Apagado = 0
AND Movimento.Situacao_Expedicao <> 'A'
AND Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
AND Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
AND Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF', 'DEV', 'CVE')
AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
AND (Funcionarios.Ordem = @Vendedor OR @Vendedor IS NULL)
AND Movimento_Prod_Serv.Linha_Excluida = 0
GROUP BY
Prod_Serv.Codigo,
Prod_Serv.Nome,
Funcionarios.Codigo,
Funcionarios.Apelido 
ORDER BY 
Funcionarios.Codigo,
Valor_Produto_Somado DESC
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
Prod_Serv.Codigo AS Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
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
ORDER BY 
Valor_Produto_Somado DESC
```





