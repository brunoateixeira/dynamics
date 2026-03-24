# Tabela: Prod_Serv

### Esquema
- [Prod_Serv](articles/esquemas/esquemas-prod_serv.html)

- --

## Exemplo 1: Relatório de Produtos Cadastrados

### Descrição

Retorna as informações cadastrias das mercadorias.

### Observações

Produtos inativos, bloqueados e de uso interno não devem ser apresentados.

### SQL

```sql
SELECT
Prod_Serv.Codigo AS Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
CONVERT(VARCHAR(10),Prod_Serv.Data_Cadastro,103) Data_Cadastro
FROM Prod_Serv
WHERE 
Prod_Serv.Inativo = 0
AND Prod_Serv.Bloqueado_Movimento = 0
AND Prod_Serv.Bloqueado_Venda = 0
AND Prod_Serv.Uso_Interno = 0
AND Prod_Serv.Codigo > '0'
ORDER BY
Prod_Serv.Codigo_Ordenacao
```
- --

## Exemplo 2: Relatório de Preços de Produtos

### Descrição

Retorna o valor de preço dos produtos de uma tabela de preço específica.

### Observações

A tabela dbo.Prod_Serv_Precos contém multiplos retornos para cada produto, por isso não é utilizado um JOIN convencional e sim OUTER APPLY, para evitar linhas duplicadas de mercadorias.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Tabela_1  | INT | Ordem da Tabela de Preço |

### Ligações
```SQL
OUTER APPLY 
    (SELECT Preco, Nome FROM Prod_Serv_Precos 
    INNER JOIN Tabelas_Preco ON Tabelas_Preco.Ordem = Prod_Serv_Precos.Ordem_Tabela_Preco AND Tabelas_Preco.Ordem = @Tabela_1 
    WHERE Prod_Serv_Precos.Ordem_Prod_Serv = Prod_Serv.Ordem)
```
### SQL

```sql
SELECT
Prod_Serv.Codigo AS Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
Preco_1.Preco Preco_1,
Preco_1.Nome Tabela_1
FROM Prod_Serv
OUTER APPLY 
    (SELECT Preco, Nome FROM Prod_Serv_Precos 
    INNER JOIN Tabelas_Preco ON Tabelas_Preco.Ordem = Prod_Serv_Precos.Ordem_Tabela_Preco AND Tabelas_Preco.Ordem = @Tabela_1 
    WHERE Prod_Serv_Precos.Ordem_Prod_Serv = Prod_Serv.Ordem) Preco_1
WHERE 
Prod_Serv.Inativo = 0
AND Prod_Serv.Bloqueado_Movimento = 0
AND Prod_Serv.Bloqueado_Venda = 0
AND Prod_Serv.Uso_Interno = 0
AND Prod_Serv.Codigo > '0'
ORDER BY
Prod_Serv.Codigo_Ordenacao
```
- --

## Exemplo 3: Relatório de Preços de Produtos de Várias Tabelas

### Descrição

Retorna o valor de preço dos produtos em várias tabelas de preços diferentes.

### Observações

A tabela dbo.Prod_Serv_Precos contém multiplos retornos para cada produto, por isso não é utilizado um JOIN convencional e sim OUTER APPLY, para evitar linhas duplicadas de mercadorias.

Cada tabela de preço precisa ter seu OUTER APPLY próprio.

Por conta dos multiplos OUTER APPLY, será preciso realizar o agrupamento dos campos.

### Parâmetros

| Nome | Tipo | Descrição |
|----|----|----|
| @Tabela_1  | INT | Ordem da Tabela de Preço 1 |
| @Tabela_2  | INT | Ordem da Tabela de Preço 2 |
| @Tabela_3  | INT | Ordem da Tabela de Preço 3 |
| @Tabela_4  | INT | Ordem da Tabela de Preço 4 |

### Ligações
```SQL
OUTER APPLY 
    (SELECT Preco, Nome FROM Prod_Serv_Precos 
    INNER JOIN Tabelas_Preco ON Tabelas_Preco.Ordem = Prod_Serv_Precos.Ordem_Tabela_Preco AND Tabelas_Preco.Ordem = @Tabela_1 
    WHERE Prod_Serv_Precos.Ordem_Prod_Serv = Prod_Serv.Ordem)
```
### SQL

```sql
SELECT
Prod_Serv.Codigo AS Codigo_Produto,
Prod_Serv.Nome AS Nome_Produto,
    Preco_1.Preco Preco_1,
    Preco_1.Nome Tabela_1,
    Preco_2.Preco Preco_2,
    Preco_2.Nome Tabela_2,
    Preco_3.Preco Preco_3,
    Preco_3.Nome Tabela_3,
    Preco_4.Preco Preco_4,
    Preco_4.Nome Tabela_4
FROM Prod_Serv
    OUTER APPLY 
    (SELECT Preco, Nome FROM Prod_Serv_Precos 
    INNER JOIN Tabelas_Preco ON Tabelas_Preco.Ordem = Prod_Serv_Precos.Ordem_Tabela_Preco AND Tabelas_Preco.Ordem = @Tabela_1 
    WHERE Prod_Serv_Precos.Ordem_Prod_Serv = PS.Ordem) Preco_1 
    OUTER APPLY 
    (SELECT Preco, Nome FROM Prod_Serv_Precos 
    INNER JOIN Tabelas_Preco ON Tabelas_Preco.Ordem = Prod_Serv_Precos.Ordem_Tabela_Preco AND Tabelas_Preco.Ordem = @Tabela_2
    WHERE Prod_Serv_Precos.Ordem_Prod_Serv = PS.Ordem) Preco_2
    OUTER APPLY 
    (SELECT Preco, Nome FROM Prod_Serv_Precos 
    INNER JOIN Tabelas_Preco ON Tabelas_Preco.Ordem = Prod_Serv_Precos.Ordem_Tabela_Preco AND Tabelas_Preco.Ordem = @Tabela_3
    WHERE Prod_Serv_Precos.Ordem_Prod_Serv = PS.Ordem) Preco_3
    OUTER APPLY 
    (SELECT Preco, Nome FROM Prod_Serv_Precos 
    INNER JOIN Tabelas_Preco ON Tabelas_Preco.Ordem = Prod_Serv_Precos.Ordem_Tabela_Preco AND Tabelas_Preco.Ordem = @Tabela_4
    WHERE Prod_Serv_Precos.Ordem_Prod_Serv = PS.Ordem) Preco_4
WHERE 
Prod_Serv.Inativo = 0
AND Prod_Serv.Bloqueado_Movimento = 0
AND Prod_Serv.Bloqueado_Venda = 0
AND Prod_Serv.Uso_Interno = 0
AND Prod_Serv.Codigo > '0'
GROUP BY 
    PS.Codigo_Ordenacao,
    PS.Codigo,
    PS.Nome,
    Preco_1.Preco ,
    Preco_1.Nome ,
    Preco_2.Preco ,
    Preco_2.Nome ,
    Preco_3.Preco ,
    Preco_3.Nome ,
    Preco_4.Preco ,
    Preco_4.Nome 
ORDER BY
Prod_Serv.Codigo_Ordenacao
```
- --