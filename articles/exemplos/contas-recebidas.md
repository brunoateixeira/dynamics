# Tabelas: Financeiro_Contas

### Esquema
- [Financeiro_Contas](articles/esquemas/esquemas-financeiro_contas.html)

- --
## Exemplo 1: Relatório de Forma de Recebimento de Contas Recebidas

### Descrição

Exibe o tipo original da conta e como ela foi recebida.

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
END AS Tipo_Conta,
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
Codigo_Sequencia, Tipo_Conta, Parcela

```