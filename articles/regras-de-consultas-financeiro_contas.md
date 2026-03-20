# Regras de Consultas e Negócios
- --
Considere as regras abaixo como obrigatórias.

## Regras para Relatórios de Vendas

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
## Regras para os Filtros nos Relatórios de Vendas

As seguintes regras serão aplicadas ao utilizar a cláusula WHERE nos Relatórios de Vendas.

```sql
Movimento.Apagado = 0 
```
Precisa ser utilizado para não mostrar vendas apagadas no sistema.

```sql
Movimento.Situacao_Expedicao <> 'A' 
```
Venda com "Expedição em Aberto" são vendas que ainda não estão concluídas e, por isso, não devem ser apresentadas.

```sql
(Filiais.Ordem = @Filial OR @Filial IS NULL)
```

O filtro de filial permite ver a venda de cada loja individualmente. Porém, o "OR @Filial IS NULL" permite ver a venda de todas as lojas se desejado.

Quando na solicitação for explícito o código das filiais (Ex: "Quero relatório das filiais 1 e 2...") deve ser utilizado o campo Filiais.Codigo. 

```sql
Movimento.Data_Passou_Desefetivacao_Estoque IS NULL
```
Vendas "Desefetivadas" são vendas desfeitas e não devem aparecer. Se houver data de desefetivação, então a venda foi desfeita.

```sql
Movimento.Data_Passou_Efetivacao_Estoque >= @Data_Inicial
Movimento.Data_Passou_Efetivacao_Estoque < DATEADD(DAY,1,@Data_Final)
```
Deve haver um período de datas para a consulta, para que não puxe uma quantidade desnecessária de dados.

A data final sempre será apenas menor (e não menor igual) "<", pois estará acompanhado da função DATEADD(DAY,1,@Data_Final). Isto é necessário por conta da estrutura do banco de dados que carimba a hora final como meia-noite do dia apontado. 

```sql
Movimento_Prod_Serv.Linha_Excluida = 0 
```

Itens excluídos são itens que são removidos ou relançados nas vendas, eles não foram efetivamente vendidos.

Essa linha deve ser adicionada apenas se houver produtos na consulta ou quando a tabela for ligada na consulta.

- --
## Tratamento de devoluções e correções

As consultas de venda devem considerar devoluções por padrão:

- Inclua no filtro: AND Movimento.Tipo_Operacao IN ('VND','VPC','VEF','DEV','CVE')
- Em colunas de valor e quantidade, aplique sinal negativo para devoluções:

```sql
CASE WHEN Movimento.Tipo_Operacao IN ('VND','VPC','VEF')
     THEN coluna_de_valor
     ELSE -coluna_de_valor
END
```
- --
## Tipos de Operações de Movimentação


