# Tabelas: Financeiro_Contas

### Esquema
- [Financeiro_Contas](articles/esquemas/esquemas-financeiro_contas.html)

- --
## Exemplo 1: Relatório de Recebíveis Baixados

### Descrição

Exibe os recebimentos baixados pela data de quitação. Inclui todos os tipos de contas.

### Observação
### Coluna: Tipo_Conta
“A” = “Conta” |
“B” = “Boleto” |
“N” = “Carnê” |
“R” = “Carteira” |
“C” = “Cartão Crédito” |
“D” = “Cartão Débito” |
“O” = “Cartão Convênio” |
“H” = “Cheque” |
“E” = “Conta Caderno” |
“F” = “Diferença de Contas” |
“G” = “Crédito Gerado Entrada/Saída” |



### Parâmetros

| Nome | Tipo | Descrição |
|------|------|-----------|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Ordem da Filial |

### Ligações
```sql
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
```

### SQL
```sql
SELECT
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Emissao,103) Data_Emissao,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Vencimento,103) Data_Vencimento,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Quitacao,103) Data_Quitacao,
Financeiro_Contas.Valor_Base AS Valor_Bruto,
Financeiro_Contas.Valor_Base + Financeiro_Contas.Valor_Juros + Financeiro_Contas.Valor_Multa - Financeiro_Contas.Valor_Desconto AS Valor_Líquido,
Financeiro_Contas.Valor_Quitado AS Valor_Quitado
FROM Financeiro_Contas
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
WHERE 1 = 1
	AND Financeiro_Contas.Data_Quitacao >= @Data_Inicial
	AND Financeiro_Contas.Data_Quitacao < DATEADD(D,1,@Data_Final)
	AND (Financeiro_Contas.Ordem_Filial = @Filial OR @Filial = 0)
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Tipo_Conta IN ('A','B','C','D','O','N','H','R')
	AND Financeiro_Contas.Pagar_Receber = 'R'
	
    
	AND Financeiro_Contas.Tipo_Recebido_Pago <> 'T'
	 AND (
	 Financeiro_Contas.Tipo_Conta NOT IN  ('A')
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A')
		AND Financeiro_Contas.Tipo_Recebido_Pago = 'C'
		AND Financeiro_Contas.Ordem_Pai IS NOT NULL
		AND EXISTS (SELECT 1 FROM Financeiro_Contas FC WHERE FC.Ordem_Renegociado_Agrupado = Financeiro_Contas.Ordem_Pai 
		AND fc.Tipo_Conta IN ('B','C','D','O','N','H','R'))
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A') AND Financeiro_Contas.Tipo_Recebido_Pago = 'C' AND Financeiro_Contas.Tela_Origem = 15)
	 )) 
ORDER BY 
CONVERT(DATE,Financeiro_Contas.Data_Emissao,103),
CONVERT(DATE,Financeiro_Contas.Data_Quitacao,103),
CONVERT(DATE,Financeiro_Contas.Data_Vencimento,103)
```

## Exemplo 2: Relatório de Recebíveis com Tipo de Conta

### Descrição

Exibe o tipo do recebível que foi quitado. 

### Parâmetros

| Nome | Tipo | Descrição |
|------|------|-----------|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Ordem da Filial |

### Ligações
```sql
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
```

### SQL

```sql
SELECT
Financeiro_Contas.Codigo_Sequencia,
CASE Financeiro_Contas.Tipo_Conta
WHEN 'A' THEN 'Conta'
WHEN 'B' THEN 'Boleto'
WHEN 'N' THEN 'Carnê'
WHEN 'R' THEN 'Carteira'
WHEN 'C' THEN 'Cartão Crédito'
WHEN 'D' THEN 'Cartão Débito'
WHEN 'O' THEN 'Cartão Convênio'
WHEN 'H' THEN 'Cheque'
WHEN 'E' THEN 'Conta Caderno'
WHEN 'F' THEN 'Diferença de Contas'
WHEN 'G' THEN 'Crédito Gerado Entrada/Saída'
ELSE Financeiro_Contas.Tipo_Conta 
END AS Tipo_Conta
FROM Financeiro_Contas
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
WHERE 1 = 1
	AND Financeiro_Contas.Data_Quitacao >= @Data_Inicial
	AND Financeiro_Contas.Data_Quitacao < DATEADD(D,1,@Data_Final)
	AND (Financeiro_Contas.Ordem_Filial = @Filial OR @Filial = 0)
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Tipo_Conta IN ('A','B','C','D','O','N','H','R')
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Situacao IN ('F','Q','I','X')
	AND Financeiro_Contas.Tipo_Recebido_Pago <> 'T'
	 AND (
	 Financeiro_Contas.Tipo_Conta NOT IN  ('A')
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A')
		AND Financeiro_Contas.Tipo_Recebido_Pago = 'C'
		AND Financeiro_Contas.Ordem_Pai IS NOT NULL
		AND EXISTS (SELECT 1 FROM Financeiro_Contas FC WHERE FC.Ordem_Renegociado_Agrupado = Financeiro_Contas.Ordem_Pai 
		AND fc.Tipo_Conta IN ('B','C','D','O','N','H','R'))
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A') AND Financeiro_Contas.Tipo_Recebido_Pago = 'C' AND Financeiro_Contas.Tela_Origem = 15)
	 )) 
ORDER BY 
Codigo_Sequencia, Tipo_Conta
```

## Exemplo 3: Relatório de Recebíveis com Forma de Recebimento

### Descrição

Exibe em qual forma de pagamento o recebível foi quitado.

### Parâmetros

| Nome | Tipo | Descrição |
|------|------|-----------|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Ordem da Filial |

### Ligações
```sql
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
```

### SQL

```sql
SELECT
Financeiro_Contas.Codigo_Sequencia,
CASE Financeiro_Contas.Tipo_Recebido_Pago
WHEN 'A' THEN 'Várias Contas'
WHEN 'D' THEN 'Dinheiro'
WHEN 'B' THEN 'Depósito'
WHEN 'C' THEN 'Crédito'
WHEN 'T' THEN 'Troca de Produto'
WHEN 'H' THEN 'Cheque de Cliente'
WHEN 'V' THEN 'Vale'
WHEN 'F' THEN 'Descontado na Fonte'
WHEN 'R' THEN 'Conta Caderno'
ELSE Financeiro_Contas.Tipo_Recebido_Pago
END AS Recebido_Em,
Financeiro_Contas.Parcela_Descricao AS Parcela
FROM Financeiro_Contas
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
WHERE 1 = 1
	AND Financeiro_Contas.Data_Quitacao >= @Data_Inicial
	AND Financeiro_Contas.Data_Quitacao < DATEADD(D,1,@Data_Final)
	AND (Financeiro_Contas.Ordem_Filial = @Filial OR @Filial = 0)
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Tipo_Conta IN ('A','B','C','D','O','N','H','R')
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Situacao IN ('F','Q','I','X')
	AND Financeiro_Contas.Tipo_Recebido_Pago <> 'T'
	 AND (
	 Financeiro_Contas.Tipo_Conta NOT IN  ('A')
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A')
		AND Financeiro_Contas.Tipo_Recebido_Pago = 'C'
		AND Financeiro_Contas.Ordem_Pai IS NOT NULL
		AND EXISTS (SELECT 1 FROM Financeiro_Contas FC WHERE FC.Ordem_Renegociado_Agrupado = Financeiro_Contas.Ordem_Pai 
		AND fc.Tipo_Conta IN ('B','C','D','O','N','H','R'))
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A') AND Financeiro_Contas.Tipo_Recebido_Pago = 'C' AND Financeiro_Contas.Tela_Origem = 15)
	 )) 
ORDER BY 
Codigo_Sequencia, Tipo_Recebido_Pago
```

## Exemplo 4: Relatório de Recebíveis com Clientes

### Descrição

Exibe as informações de recebíveis com os dados do cliente do recebimento.

### Parâmetros

| Nome | Tipo | Descrição |
|------|------|-----------|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Ordem da Filial |
| @Cliente | INT | Ordem do Cliente |

### Ligações
```sql
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
INNER JOIN Cli_For ON Cli_For.Ordem = Financeiro_Contas.Ordem_Cli_For
```

### SQL

```sql
SELECT
Cli_For.Codigo AS Codigo_Cliente,
Cli_For.Nome AS Nome_Cliente,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Emissao,103) Data_Emissao,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Vencimento,103) Data_Vencimento,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Quitacao,103) Data_Quitacao,
Financeiro_Contas.Valor_Base AS Valor_Bruto,
Financeiro_Contas.Valor_Base + Financeiro_Contas.Valor_Juros + Financeiro_Contas.Valor_Multa - Financeiro_Contas.Valor_Desconto AS Valor_Líquido,
Financeiro_Contas.Valor_Quitado AS Valor_Quitado
FROM Financeiro_Contas
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
INNER JOIN Cli_For ON Cli_For.ordem = Financeiro_Contas.Ordem_Cli_For
WHERE 1 = 1
	AND Financeiro_Contas.Data_Quitacao >= @Data_Inicial
	AND Financeiro_Contas.Data_Quitacao < DATEADD(D,1,@Data_Final)
	AND (Financeiro_Contas.Ordem_Filial = @Filial OR @Filial = 0)
	AND (Financeiro_Contas.Ordem_Cli_For = @Cliente OR @Cliente = 0)
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Tipo_Conta IN ('A','B','C','D','O','N','H','R')
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Situacao IN ('F','Q','I','X')
	AND Financeiro_Contas.Tipo_Recebido_Pago <> 'T'
	 AND (
	 Financeiro_Contas.Tipo_Conta NOT IN  ('A')
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A')
		AND Financeiro_Contas.Tipo_Recebido_Pago = 'C'
		AND Financeiro_Contas.Ordem_Pai IS NOT NULL
		AND EXISTS (SELECT 1 FROM Financeiro_Contas FC WHERE FC.Ordem_Renegociado_Agrupado = Financeiro_Contas.Ordem_Pai 
		AND fc.Tipo_Conta IN ('B','C','D','O','N','H','R'))
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A') AND Financeiro_Contas.Tipo_Recebido_Pago = 'C' AND Financeiro_Contas.Tela_Origem = 15)
	 )) 
ORDER BY 
Cli_For.Codigo,
CONVERT(DATE,Financeiro_Contas.Data_Emissao,103),
CONVERT(DATE,Financeiro_Contas.Data_Quitacao,103),
CONVERT(DATE,Financeiro_Contas.Data_Vencimento,103)
```

## Exemplo 5: Relatório de Recebíveis com Vendedor

### Descrição

Exibe as informações de recebíveis com os dados do vendedor do recebimento.

### Parâmetros

| Nome | Tipo | Descrição |
|------|------|-----------|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Ordem da Filial |
| @Vendedor | INT | Ordem do Vendedor |

### Ligações
```sql
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
INNER JOIN Funcionarios ON Funcionarios.Ordem = Financeiro_Contas.Ordem_Vendedor1
```

### SQL

```sql
SELECT
Funcionarios.Codigo AS Codigo_Vendedor,
Funcionarios.Apelido AS Nome_Vendedor,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Emissao,103) Data_Emissao,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Vencimento,103) Data_Vencimento,
CONVERT(VARCHAR(10),Financeiro_Contas.Data_Quitacao,103) Data_Quitacao,
Financeiro_Contas.Valor_Base AS Valor_Bruto,
Financeiro_Contas.Valor_Base + Financeiro_Contas.Valor_Juros + Financeiro_Contas.Valor_Multa - Financeiro_Contas.Valor_Desconto AS Valor_Líquido,
Financeiro_Contas.Valor_Quitado AS Valor_Quitado
FROM Financeiro_Contas
INNER JOIN Filiais ON Financeiro_Contas.Ordem_Filial = Filiais.Ordem
INNER JOIN Funcionarios ON Funcionarios.Ordem = Financeiro_Contas.Ordem_Vendedor1
WHERE 1 = 1
	AND Financeiro_Contas.Data_Quitacao >= @Data_Inicial
	AND Financeiro_Contas.Data_Quitacao < DATEADD(D,1,@Data_Final)
	AND (Financeiro_Contas.Ordem_Filial = @Filial OR @Filial = 0)
	AND (Funcionarios.Ordem = @Vendedor OR @Vendedor = 0)
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Tipo_Conta IN ('A','B','C','D','O','N','H','R')
	AND Financeiro_Contas.Pagar_Receber = 'R'
	AND Financeiro_Contas.Situacao IN ('F','Q','I','X')
	AND Financeiro_Contas.Tipo_Recebido_Pago <> 'T'
	 AND (
	 Financeiro_Contas.Tipo_Conta NOT IN  ('A')
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A')
		AND Financeiro_Contas.Tipo_Recebido_Pago = 'C'
		AND Financeiro_Contas.Ordem_Pai IS NOT NULL
		AND EXISTS (SELECT 1 FROM Financeiro_Contas FC WHERE FC.Ordem_Renegociado_Agrupado = Financeiro_Contas.Ordem_Pai 
		AND fc.Tipo_Conta IN ('B','C','D','O','N','H','R'))
		OR (
		Financeiro_Contas.Tipo_Conta IN ('A') AND Financeiro_Contas.Tipo_Recebido_Pago = 'C' AND Financeiro_Contas.Tela_Origem = 15)
	 )) 
ORDER BY 
Funcionarios.Codigo,
CONVERT(DATE,Financeiro_Contas.Data_Emissao,103),
CONVERT(DATE,Financeiro_Contas.Data_Quitacao,103),
CONVERT(DATE,Financeiro_Contas.Data_Vencimento,103)
```