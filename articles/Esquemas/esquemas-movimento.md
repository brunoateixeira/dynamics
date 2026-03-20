# Esquema Tabela dbo.Movimento

Contém os dados principais e totalizados das vendas.

### Regra para Campos com Três Versões:

Alguns campos terão três versões: 
Total do campo em Produtos
Total do campo em Serviços
Total do campo somando Produtos e Serviços

A prioridade de uso é sempre o campo que some ambos Produtos e Serviços.

Exemplo:
Qtde_Total_Geral
Qtde_Total_Prod
Qtde_Total_Serv

A prioridade de uso nesse exemplo seria Qtde_Total_Serv.

- --

### Campos para Ligações (Chave Primária e Chaves Estrangeiras)

Estes campos serão utilizados para ligar outras tabelas no banco de dados.

| Coluna | Tipo de Dados | Permite Nulo | Tabela Origem |
|--------|---------------|--------------|---------------|
| Entrega_Parcial_Ordem_Operacao_Anterior | int | SIM | dbo.Operacoes |
| Ordem (PK) | int | NÃO | ? |
| Ordem_Caixa | int | NÃO | dbo.Caixas |
| Ordem_Centro_Custo | int | SIM | dbo.Centro_Custo |
| Ordem_Cli_For | int | NÃO | dbo.Cli_For |
| Ordem_Comissionado | int | NÃO | dbo.Cli_For |
| Ordem_Credito_Cli_For | int | NÃO | dbo.Credito_Cli_For |
| Ordem_Filial | int | NÃO | dbo.Filiais |
| Ordem_Financeiro | int | NÃO | ? |
| Ordem_Financeiro_Conta_Frete_Emitente | int | SIM | ? |
| Ordem_Financeiro_Frete | int | NÃO | ? |
| Ordem_Funcionario_Alteracao | int | NÃO | dbo.Funcionarios |
| Ordem_Funcionario_Cancelou_Devolucao_Simplificada | int | SIM | dbo.Funcionarios |
| Ordem_Funcionario_Checkout | int | NÃO | dbo.Funcionarios |
| Ordem_Funcionario_Conferencia_Confirmou | int | SIM | dbo.Funcionarios |
| Ordem_Funcionario_Desefetivacao | int | SIM | dbo.Funcionarios |
| Ordem_Funcionario_Efetivacao_Estoque | int | SIM | dbo.Funcionarios |
| Ordem_Funcionario_Efetivacao_Financeiro | int | SIM | dbo.Funcionarios |
| Ordem_Funcionario_Imprimiu_Ticket | int | SIM | dbo.Funcionarios |
| Ordem_Funcionario_Liberacao | int | SIM | dbo.Funcionarios |
| Ordem_Funcionario_Processou_Devolucao_Simplificada | int | SIM | dbo.Funcionarios |
| Ordem_Intermediador | int | SIM | dbo.Intermediadores |
| Ordem_Motivo_Cancelamento_Orcamento | int | SIM | dbo.Motivo_Cancelamento_Orcamento |
| Ordem_Motivo_Desefetivacao | int | SIM | dbo.Motivo_Desefetivacao |
| Ordem_Movimento_Referenciado | int | SIM | dbo.Movimento |
| Ordem_Operacao | int | NÃO | dbo.Operacoes |
| Ordem_Operador | int | NÃO | dbo.Funcionarios |
| Ordem_Origem_Venda | int | SIM | dbo.Origem_Vendas |
| Ordem_Plano_Contas3 | int | SIM | dbo.Plano_Contas3 |
| Ordem_Plano_Contas3_Parcelas_Saida | int | SIM | dbo.Plano_Contas3 |
| Ordem_Rec_Pag | int | NÃO | ? |
| Ordem_Tabela_Custo | int | SIM | dbo.Tabelas_Preco |
| Ordem_Tabela_Custo_Medio | int | SIM | dbo.Tabelas_Preco |
| Ordem_Tabela_Preco | int | NÃO | dbo.Tabelas_Preco |
| Ordem_Vendedor1 | int | NÃO | dbo.Funcionarios |
| Ordem_Vendedor2 | int | NÃO | dbo.Funcionarios |

### Campos de Data

#### Regra para Datas
Os campo de data devem sempre ser convertido em DD/MM/AAAA, utilizando a função CONVERT(VARCHAR(10),Campo_de_Data,103).

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Data | datetime | SIM | Data da última gravação do Movimento (essa data deve ser priorizada em relatórios de Orçamentos e relatórios de movimentações não efetivadas) |
| Data_Acerto_Emprestimo | datetime | SIM | Data do Acerto da Consignação |
| Data_Alteracao | datetime | SIM | Data da última alteração do Movimento |
| Data_Conferencia_Confirmou | datetime | SIM | Data de quando a Conferência foi realizada |
| Data_Criacao | datetime | SIM | Data de Criação do Movimento |
| Data_Desefetivado_Estoque | datetime | SIM | Data quando o Estoque foi Desefetivado |
| Data_Desefetivado_Financeiro | datetime | SIM | Data quando o Financeiro foi Desefetivado |
| Data_Efetivado_Estoque | datetime | SIM | Data quando o Estoque foi Efetivado |
| Data_Efetivado_Financeiro | datetime | SIM | Data quando o Financeiro foi Efetivado |
| Data_Entrega | datetime | SIM | ? |
| Data_Imprimiu_Ticket | date | SIM | Data que o Ticket foi Impresso na Saída |
| Data_Passou_Desefetivacao_Estoque | datetime | SIM | Data de quando o Movimento foi Desefetivado|
| Data_Passou_Efetivacao_Estoque | datetime | SIM | Data que considera primeiro a data de efetivação do estoque e, se não houver movimento de estoque, considera a data de efetivação de financeiro (essa data é deve ser priorizada em relatórios de vendas) |
| Data_Previsao_Recebimento | datetime | SIM | ? |
| Data_Sincronizacao | datetime | SIM | Data de quando a venda foi Sincronizada de módulos externos |

### Campos de Valores

Os campos de valores da tabela dbo.Movimento sempre serão a soma de todas as mercadorias da movimentação. Se refere as totalizações da movimentação.

Deve ser respeitado a Regra para Campos com Três Versões.

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Acrescimo_Valor_Somado | money | NÃO | Usado apenas em Entradas Importadas do Exterior. Há um campo próprio chamado "Acréscimo" |
| Cubagem_Final | money | NÃO | Cubagem Total das Mercadorias |
| Desconto_Total_Prod | money | NÃO | Desconto Total só para Produtos |
| Desconto_Total_Serv | money | NÃO | Desconto Total só para Serviços |
| Desconto_Valor_Somado | money | NÃO | Desconto Total para Produtos e Serviços (Esse campo deve ser priorizado para mostrar o desconto total da movimentação) |
| Despesa_Acessoria | money | NÃO | Valor Total de Despesas Acessórias |
| Frete_Valor_Separado_Somado | money | NÃO | Valor de Frete pago à parte, não incluso no total da Movimentação |
| Frete_Valor_Somado | money | NÃO | Valor de Frete incluso no total na Movimentação (Esse campo deve ser priorizado para mostrar o total de frte da movimentação) |
| Peso_Final | money | NÃO | Peso Bruto Total das Mercadorias  |
| Peso_Liquido_Final | money | NÃO | Peso Líquido Total das Mercadorias  |
| Preco_Custo_Medio_Somado | money | NÃO | Custo Médio Total das mercadorias movimentadas |
| Preco_Custo_Somado | money | NÃO | Custo Total das mercadorias movimentadas |
| Preco_Final_Somado | money | NÃO | Valor Líquido. Valor Total da Movimentação. Considera valor dos itens + impostos - descontos. (Esse campo deve ser somado para mostrar o Valor Total de movimentações) |
| Preco_Total_Com_Desconto_Somado | money | NÃO | Valor Total de Produtos e Serviços com Desconto considerado |
| Preco_Total_Sem_Desconto_Somado | money | NÃO | Valor Bruto. Valor Total de Produtos e Serviços sem Desconto considerado |
| Preco_Total_Prod | money | NÃO | Valor Total de Produtos com Desconto considerado |
| Preco_Total_Prod_Sem_Desconto_Somado | money | NÃO | Valor Total de Produtos sem Desconto considerado |
| Preco_Total_Serv | money | NÃO | Valor Total de Serviços com Desconto considerado |
| Preco_Total_Serv_Sem_Desconto_Somado | money | NÃO | Valor Total de Serviços sem Desconto considerado |
| Qtde_Total_Geral | money | NÃO | Quantidade Total em Unidades de Produtos e Serviços (Esse campo deve ser priorizado para mostrar a quantidade total de itens da movimentação) |
| Qtde_Total_Prod | money | NÃO | Quantidade Total em Unidades só de Produtos |
| Qtde_Total_Serv | money | NÃO | Quantidade Total em Unidades só de Serviços |
| Valor_Container | money | SIM | Utilizado apenas em Saídas de Exportação ao Exterior |
| Valor_Pagamento_Ecommerce_Integracoes | money | SIM | Utilizado em Movimentações que foram criadas pelo módulo Integrações |

### Campos de Impostos

Valores individuais de cada imposto da movimentação. 

Esses valores já estão inclusos no campo Preco_Final_Somado e não precisar ser somados à ele novamente.

Deve ser respeitado a Regra para Campos com Três Versões.

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| CBS_Base_Somado | money | SIM | |
| CBS_CredPres_CondSus_Valor_Somado | money | SIM | |
| CBS_CredPres_Valor_Somado | money | SIM | |
| CBS_Devolucao_Tributos_Valor_Somado | money | SIM | |
| CBS_Diferido_Valor_Somado | money | SIM | |
| CBS_Monofasico_Retencao_Valor_Somado | money | SIM | |
| CBS_Monofasico_Retido_Valor_Somado | money | SIM | |
| CBS_Monofasico_Valor_Somado | money | SIM | |
| CBS_Valor_Somado | money | SIM | |
| Carga_Tributaria | money | NÃO | |
| Carga_Tributaria_Estadual | money | NÃO | |
| Carga_Tributaria_Federal | money | NÃO | |
| Carga_Tributaria_Municipal | money | NÃO | |
| Codigo_MunIBS | varchar | SIM | |
| COFINS_Normal_Base_Prod_Somado | money | NÃO | |
| COFINS_Normal_Base_Serv_Somado | money | NÃO | |
| COFINS_Normal_Base_Somado | money | NÃO | |
| COFINS_Normal_Valor_Prod_Somado | money | NÃO | |
| COFINS_Normal_Valor_Serv_Somado | money | NÃO | |
| COFINS_Normal_Valor_Somado | money | NÃO | |
| COFINS_Subst_Base_Prod_Somado | money | NÃO | |
| COFINS_Subst_Base_Serv_Somado | money | NÃO | |
| COFINS_Subst_Base_Somado | money | NÃO | |
| COFINS_Subst_Valor_Prod_Somado | money | NÃO | |
| COFINS_Subst_Valor_Serv_Somado | money | NÃO | |
| COFINS_Subst_Valor_Somado | money | NÃO | |
| COFINS_Subst_Valor_Somado_Total | money | SIM | |
| CSOSN_Base_Somado | money | NÃO | |
| CSOSN_Valor_Somado | money | NÃO | |
| FCP_Normal_Base_Somado | money | SIM | |
| FCP_Normal_Valor_Somado | money | SIM | |
| FCP_Retido_Base_Somado | money | SIM | |
| FCP_Retido_Valor_Converteu_CST_60_XML_Somado | money | SIM | |
| FCP_Retido_Valor_Somado | money | SIM | |
| FCP_ST_Base_Somado | money | SIM | |
| FCP_ST_Valor_Somado | money | SIM | |
| FET_Base | money | SIM | |
| FET_Valor | money | SIM | |
| IBS_Est_Base_Somado | money | SIM | |
| IBS_Est_Devolucao_Tributos_Valor_Somado | money | SIM | |
| IBS_Est_Diferido_Valor_Somado | money | SIM | |
| IBS_Est_Valor_Somado | money | SIM | |
| IBS_Monofasico_Retencao_Valor_Somado | money | SIM | |
| IBS_Monofasico_Retido_Valor_Somado | money | SIM | |
| IBS_Monofasico_Valor_Somado | money | SIM | |
| IBS_Mun_Base_Somado | money | SIM | |
| IBS_Mun_CredPres_CondSus_Valor_Somado | money | SIM | |
| IBS_Mun_CredPres_Valor_Somado | money | SIM | |
| IBS_Mun_Devolucao_Tributos_Valor_Somado | money | SIM | |
| IBS_Mun_Diferido_Valor_Somado | money | SIM | |
| IBS_Mun_Valor_Somado | money | SIM | |
| IBS_Valor_Somado | money | SIM | |
| ICMS_Base_Efetivo_Somado | money | SIM | |
| ICMS_Desonerado_Valor_Somado | money | NÃO | |
| ICMS_Desonerado_Valor_Subtrair_Total_Somado | money | NÃO | |
| ICMS_Diferido_Base_Somado | money | NÃO | |
| ICMS_Diferido_Normal_Valor_Somado | money | NÃO | |
| ICMS_Diferido_Valor_Diferimento_Somado | money | NÃO | |
| ICMS_Diferido_Valor_Operacao_Somado | money | NÃO | |
| ICMS_FCP_Valor_Somado | money | SIM | |
| ICMS_Monofasico_Normal_Base_Somado | money | SIM | |
| ICMS_Monofasico_Normal_Valor_Somado | money | SIM | |
| ICMS_Monofasico_Retencao_Base_Somado | money | SIM | |
| ICMS_Monofasico_Retencao_Valor_Somado | money | SIM | |
| ICMS_Monofasico_Retido_Base_Somado | money | SIM | |
| ICMS_Monofasico_Retido_Valor_Somado | money | SIM | |
| ICMS_Normal_Base_Somado | money | NÃO | |
| ICMS_Normal_Valor_Somado | money | NÃO | |
| ICMS_Partilhado_Base_Somado | money | SIM | |
| ICMS_Partilhado_Valor_Destino_Somado | money | SIM | |
| ICMS_Partilhado_Valor_Remetente_Somado | money | SIM | |
| ICMS_Retido_Base_Somado | money | NÃO | |
| ICMS_Retido_Valor_Somado | money | NÃO | |
| ICMS_Simples_Base_Somado | money | NÃO | |
| ICMS_Simples_Valor_Somado | money | NÃO | |
| ICMS_Subst_Base_Somado | money | NÃO | |
| ICMS_Subst_Valor_Somado | money | NÃO | |
| ICMS_Valor_Efetivo_Somado | money | SIM | |
| II_Base_Somado | money | NÃO | |
| II_Valor_Somado | money | NÃO | |
| INSS_Base | money | SIM | |
| INSS_Valor | money | SIM | |
| INSS_Valor_Subtrair_Total_Somado | money | SIM | |
| IPI_Base_Somado | money | NÃO | |
| IPI_Valor_Devolvido_Somado | money | SIM | |
| IPI_Valor_Somado | money | NÃO | |
| IR_Base | money | NÃO | |
| IR_Valor | money | NÃO | |
| IS_Base_Somado | money | SIM | |
| IS_Valor_Somado | money | SIM | |
| ISS_Base | money | NÃO | |
| ISS_Valor | money | NÃO | |
| ISS_Valor_Sem_Arredondar | money | SIM | |
| ISS_Valor_Subtrair_Total_Somado | money | SIM | |
| PIS_Normal_Base_Prod_Somado | money | NÃO | |
| PIS_Normal_Base_Serv_Somado | money | NÃO | |
| PIS_Normal_Base_Somado | money | NÃO | |
| PIS_Normal_Valor_Prod_Somado | money | NÃO | |
| PIS_Normal_Valor_Serv_Somado | money | NÃO | |
| PIS_Normal_Valor_Somado | money | NÃO | |
| PIS_Subst_Base_Prod_Somado | money | NÃO | |
| PIS_Subst_Base_Serv_Somado | money | NÃO | |
| PIS_Subst_Base_Somado | money | NÃO | |
| PIS_Subst_Valor_Prod_Somado | money | NÃO | |
| PIS_Subst_Valor_Serv_Somado | money | NÃO | |
| PIS_Subst_Valor_Somado | money | NÃO | |
| PIS_Subst_Valor_Somado_Total | money | SIM | |
| Retencao_COFINS | money | NÃO | |
| Retencao_CSLL | money | NÃO | |
| Retencao_INSS | money | SIM | |
| Retencao_INSS_Aliquota | money | SIM | |
| Retencao_PIS | money | NÃO | |
| Retencao_Produto_COFINS_Base_Somado | money | SIM | |
| Retencao_Produto_COFINS_Valor_Somado | money | SIM | |
| Retencao_Produto_CSLL_Base_Somado | money | SIM | |
| Retencao_Produto_CSLL_Valor_Somado | money | SIM | |
| Retencao_Produto_IR_Base_Somado | money | SIM | |
| Retencao_Produto_IR_Valor_Somado | money | SIM | |
| Retencao_Produto_PIS_Base_Somado | money | SIM | |
| Retencao_Produto_PIS_Valor_Somado | money | SIM | |

### Campos Gerais

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Alteracoes_Offline | bit | NÃO | |
| Apagado | bit | NÃO | Utilizado para localizar saídas apagadas |
| Arquivo_XML_Importado | varchar | NÃO | |
| Bipagem_Efetuada | bit | SIM | |
| Bipagem_Liberada | bit | SIM | |
| Cidade_Prestacao | varchar | SIM | |
| Cidade_Recolhimento | varchar | SIM | |
| Comissao_Perc | money | NÃO | |
| Conferencia_Confirmou | bit | SIM | |
| CRMBONUS_Acumulou | bit | SIM | |
| CRMBONUS_Cancelou | bit | SIM | |
| CRMBONUS_Celular | varchar | SIM | |
| CRMBONUS_Flag_Acumular_Bonus | bit | SIM | |
| CRMBONUS_ID_Bonus | varchar | SIM | |
| CRMBONUS_ID_Bonus_Utilizado | varchar | SIM | |
| CRMBONUS_ID_Cliente | int | SIM | |
| CRMBONUS_Pin_Master | bit | SIM | |
| CRMBONUS_Restricao_Horario | bit | SIM | |
| CRMBONUS_SMS | varchar | SIM | |
| CRMBONUS_Utilizou | bit | SIM | |
| CRMBONUS_Valor | money | SIM | |
| Criado_Por | varchar | NÃO | Funcionário que criou o movimento. |
| Descricao_Servicos_NFSe_Interna | varchar | SIM | |
| Desefetivado_Estoque | bit | SIM | |
| Desefetivado_Financeiro | bit | NÃO | |
| Efetivado_Estoque | bit | SIM | |
| Efetivado_Financeiro | bit | NÃO | |
| Envia_NFSe | bit | SIM | |
| Entrada_Vinculou_Pedido_Compra | bit | SIM | |
| Entrega_Parcial_Gerou_Entregas | bit | SIM | |
| Entrega_Parcial_Tipo | varchar | SIM | |
| Estado_Prestacao | varchar | SIM | |
| Estado_Recolhimento | varchar | SIM | |
| FCI_Desefetivou_Qtde | bit | SIM | |
| FCI_Efetivou_Qtde | bit | SIM | |
| Gerou_Transferencias_Automaticas | bit | SIM | |
| Hora_Imprimiu_Ticket | time | SIM | Hora de impressão do Ticket |
| Indicador_Destino | varchar | NÃO | |
| Indicador_Presencial | varchar | NÃO | |
| Informacoes_Adicionais_Fisco | varchar | SIM | |
| Informacoes_Fisco | varchar | NÃO | |
| Local_Despacho | varchar | NÃO | |
| Local_Exportacao | varchar | NÃO | |
| Observacao | varchar | NÃO | |
| Observacao_Fiscal | varchar | NÃO | |
| Observacao_Liberacao | varchar | SIM | |
| Operacao_Consumidor_Final | bit | NÃO | |
| Operacao_Grava_Custo | bit | SIM | |
| Operacao_Grava_Custo_Medio | bit | SIM | |
| Operacao_Grava_Custo_Produtos_Fracionados | bit | SIM | |
| Operacao_Lotes_Tela_Expedicao | bit | SIM | |
| Operacao_Permite_Gerar_Credito_Cliente | bit | SIM | |
| Operacao_Series_Tela_Expedicao | bit | SIM | |
| Orcamento_Mantido | bit | SIM | |
| Orcamento_Venda_Passou_Liberacao_Remota_Autorizada | bit | SIM | |
| Referencia_Interna | varchar | NÃO | |
| Sequencia | int | NÃO | Campo que identifica no sistema e impressões de tickets o numero da movimentação. |
| Situacao_Expedicao | varchar | NÃO | |
| Situacao_Liberacao_Permissoes | varchar | SIM | |
| Situacao_VEF | varchar | NÃO | |
| Tipo_Operacao | varchar | NÃO | Tipo da Operação da Movimentação. Exemplos: VND, VPC, VEF |
| Tipo_Recebimento_Frete | varchar | NÃO | |
| Versao_Efetivacao_Estoque | varchar | NÃO | Versão do Shop 9. |
| Versao_Efetivacao_Financeiro | varchar | NÃO | Versão do Shop 9. |
| Versao_Gravacao | varchar | NÃO | Versão do Shop 9. |



