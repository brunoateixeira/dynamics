# Regras de Consultas e Negócios
- --
### Regras Básicas
- Data_Passou_Efetivacao_Estoque é a primeira opção para datas de venda.
- Data_Efetivado_Financeiro é a data de venda a ser considerada para considerar a data de recebimento.
- Em relatórios de venda, sempre é deduzido devoluções, a não ser que seja solicitado o contrário.
- Quando NÃO considerar devoluções use apenas ('VND','VPC','VEF') e não aplique CASE negativo.
- Vendas apagadas não são apresentadas.
- Linhas de produtos excluidos não são apresentadas.
- Vendas desefetivadas (desfeitas) não são apresentadas.
- Nem toda venda terá nota fiscal.
- Quando listados produtos na consulta, os campos de valores são da tabela Movimento_Prod_Serv e devem ser somados.
- Sempre converta campos de data em DD/MM/YYYY utilizando CONVERT(VARCHAR(10),Campo_de_Data,103).

- -- 
### Filtros obrigatórios para Vendas
- Movimento.Apagado = 0 
- Movimento.Situacao_Expedicao <> 'A' 
- (Filiais.Ordem = @Filial OR @Filial IS NULL)
- Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
- Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
- Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
- Movimento_Prod_Serv.Linha_Excluida = 0 (Caso tenha produtos na consulta)
- --
### Tratamento de devoluções e correções

As consultas de venda devem considerar devoluções por padrão:

- Inclua no filtro: AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF','DEV','CVE')
- Em colunas de valor e quantidade, aplique sinal negativo para devoluções:

```sql
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF')
     THEN coluna_de_valor
     ELSE -coluna_de_valor
END
```



