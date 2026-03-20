# Tabela: Log_V2

### Esquema
- [Log_V2](articles/esquemas/esquemas-Log_V2.html)

- --

## Exemplo 1: Relatório de Log com Produtos Excluídos

### Descrição

Retorna as informações de itens que foram excluídos das vendas.

### Observações

A dbo.Log_V2 foi utilizar porque a tabela dbo.Movimento_Prod_Serv não tem a informação de quem excluiu o produto.

Na dbo.Log_V2, os resultados são textuais, por isso foi preciso usar CHARINDEX para recortar as informações.

Por não haver informações suficientes na dbo.Log_V2, a ligação entre tabelas foi feita com ".Codigo" e não ".Ordem". Essa não é maneira recomendada de realizar as ligações.

Utilizar a dbo.Log_V2 deixa a consulta mais lenta.

Se for ligar essa informação em outra query, é recomendado deixar esta consulta como CTE.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Data_Inicial | DATE | Data Inicial |
| @Data_Final | DATE | Data Final |
| @Filial | INT | Ordem da Filial |

### Ligações
```SQL
INNER JOIN Filiais ON Filiais.Ordem = Log_V2.Ordem_Filial
INNER JOIN Funcionarios ON Funcionarios.Ordem = Log_V2.Ordem_Funcionario
```
### SQL

```sql
SELECT
Filial,
Data,
Hora,
Funcionario,
Sequencia,
Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
Quantidade,
Valor_Unitario
FROM (
    SELECT  
        Log_V2.Tela,
        Filiais.Codigo AS Filial,
        -- Código do Produto (seguro contra comprimento negativo ou não encontrado)
        CASE 
            WHEN CHARINDEX('Código ', Detalhe) > 0 
                 AND CHARINDEX('(', Detalhe, CHARINDEX('Código ', Detalhe)) > CHARINDEX('Código ', Detalhe)
            THEN TRIM(
                SUBSTRING(
                    Detalhe,
                    CHARINDEX('Código ', Detalhe) + LEN('Código '),
                    CHARINDEX('(', Detalhe, CHARINDEX('Código ', Detalhe))
                    - (CHARINDEX('Código ', Detalhe) + LEN('Código '))
                )
            )
            ELSE NULL   -- ou '' ou 'Não encontrado'
        END AS Codigo_Produto,
        -- Quantidade
        CASE 
            WHEN CHARINDEX('Quantidade: ', Detalhe) > 0 
                 AND CHARINDEX(',', Detalhe, CHARINDEX('Quantidade: ', Detalhe)) > CHARINDEX('Quantidade: ', Detalhe)
            THEN TRIM(
                SUBSTRING(
                    Detalhe,
                    CHARINDEX('Quantidade: ', Detalhe) + 12,   -- depois de "Quantidade: "
                    CHARINDEX(',', Detalhe, CHARINDEX('Quantidade: ', Detalhe)) 
                    - (CHARINDEX('Quantidade: ', Detalhe) + 12)
                )
            )
            ELSE NULL
        END AS Quantidade,
        -- Valor Unitário (R$ Unit.: xxx,xx)
        CASE 
            WHEN CHARINDEX('R$ Unit.: ', Detalhe) > 0 
                 AND CHARINDEX(')', Detalhe, CHARINDEX('R$ Unit.: ', Detalhe)) > CHARINDEX('R$ Unit.: ', Detalhe)
            THEN TRIM(
                SUBSTRING(
                    Detalhe,
                    CHARINDEX('R$ Unit.: ', Detalhe) + 10,     -- depois de "R$ Unit.: "
                    CHARINDEX(')', Detalhe, CHARINDEX('R$ Unit.: ', Detalhe)) 
                    - (CHARINDEX('R$ Unit.: ', Detalhe) + 10)
                )
            )
            ELSE NULL
        END AS Valor_Unitario,
        -- Sequência (mais tolerante – assume que vem no final)
        CASE 
            WHEN CHARINDEX('sequência ', Detalhe) > 0
            THEN TRIM(
                SUBSTRING(
                    Detalhe,
                    CHARINDEX('sequência ', Detalhe) + LEN('sequência '),
                    20   -- limite seguro
                )
            )
            ELSE NULL
        END AS Sequencia,
        CONVERT(VARCHAR(10),Log_V2.Data_Servidor,103) Data,
        CONVERT(VARCHAR(10),Log_V2.Data_Servidor,108) Hora,
        CONCAT(Funcionarios.Codigo, ' - ', Funcionarios.Apelido) as Funcionario
    FROM Log_V2
    INNER JOIN Filiais ON Filiais.Ordem = Log_V2.Ordem_Filial
    INNER JOIN Funcionarios ON Funcionarios.Ordem = Log_V2.Ordem_Funcionario
    WHERE Detalhe LIKE '%foi marcado como excluído na%'
      AND Detalhe LIKE '%Código %'   -- ajuda a filtrar linhas que realmente têm código
      AND Log_V2.Data_Servidor >= @Data_Inicial
      AND Log_V2.Data_Servidor < DATEADD(DAY,1,@Data_Final)
      AND (Filiais.Ordem = @Filial OR @Filial IS NULL)
    ) A
INNER JOIN Prod_Serv ON Prod_Serv.Codigo = A.Codigo_Produto
WHERE
Tela = 'Tela de Saídas'
ORDER BY 
Funcionario, CONVERT(DATE,DATA,103), Sequencia, Codigo_Produto
```
- --
