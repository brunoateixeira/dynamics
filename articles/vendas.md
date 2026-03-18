# Tabela: Movimento

### Esquema
- [Movimento](articles/esquemas.md)

- --

## Exemplo 1: Relatório de Vendas

### Descrição

Retorna as informações e movimentações de venda, sem abater devoluções. 

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
Movimento.Ordem_Filial = Filiais.Ordem
Movimento.Ordem_Vendedor1 = Funcionarios.Ordem
```

### SQL

```sql
SELECT
Funcionarios.Codigo  AS Codigo_Funcionário,
Funcionarios.Apelido  AS Nome_Funcionário,
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
- --
