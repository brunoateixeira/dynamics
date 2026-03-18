# Esquemas
- --
## Tabela: Movimento
- --

### Campos para Ligações

Estes campos serão utilizados para ligar outras tabelas no banco de dados.

| Coluna | Tipo de Dados | Permite Nulo | Tabela Origem |
|--------|---------------|--------------|-----------|
| Entrega_Parcial_Ordem_Operacao_Anterior | int | SIM | |
| Ordem (PK) | int | NÃO | PK |
| Ordem_Caixa | int | NÃO | |
| Ordem_Centro_Custo | int | SIM | |
| Ordem_Cli_For | int | NÃO | |
| Ordem_Comissionado | int | NÃO | |
| Ordem_Credito_Cli_For | int | NÃO | |
| Ordem_Filial | int | NÃO | |
| Ordem_Filial_CD | int | SIM | |
| Ordem_Financeiro | int | NÃO | |
| Ordem_Financeiro_Conta_Frete_Emitente | int | SIM | |
| Ordem_Financeiro_Frete | int | NÃO | |
| Ordem_Funcionario_Alteracao | int | NÃO | |
| Ordem_Funcionario_Cancelou_Devolucao_Simplificada | int | SIM | |
| Ordem_Funcionario_Checkout | int | NÃO | |
| Ordem_Funcionario_Conferencia_Confirmou | int | SIM | |
| Ordem_Funcionario_Desefetivacao | int | SIM | |
| Ordem_Funcionario_Efetivacao_Estoque | int | SIM | |
| Ordem_Funcionario_Efetivacao_Financeiro | int | SIM | |
| Ordem_Funcionario_Imprimiu_Ticket | int | SIM | |
| Ordem_Funcionario_Liberacao | int | SIM | |
| Ordem_Funcionario_Processou_Devolucao_Simplificada | int | SIM | |
| Ordem_Intermediador | int | SIM | |
| Ordem_Motivo_Cancelamento_Orcamento | int | SIM | |
| Ordem_Motivo_Desefetivacao | int | SIM | |
| Ordem_Movimento_Referenciado | int | SIM | |
| Ordem_Operacao | int | NÃO | |
| Ordem_Operador | int | NÃO | |
| Ordem_Origem_Venda | int | SIM | |
| Ordem_Plano_Contas3 | int | SIM | |
| Ordem_Plano_Contas3_Parcelas_Saida | int | SIM | |
| Ordem_Rec_Pag | int | NÃO | |
| Ordem_Tabela_Custo | int | SIM | |
| Ordem_Tabela_Custo_Medio | int | SIM | |
| Ordem_Tabela_Preco | int | NÃO | |
| Ordem_Vendedor1 | int | NÃO | |
| Ordem_Vendedor2 | int | NÃO | |

### Campos de Data

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Data | datetime | SIM | |
| Data_Acerto_Emprestimo | datetime | SIM | |
| Data_Alteracao | datetime | SIM | |
| Data_Conferencia_Confirmou | datetime | SIM | |
| Data_Criacao | datetime | SIM | |
| Data_Desefetivado_Estoque | datetime | SIM | |
| Data_Desefetivado_Financeiro | datetime | SIM | |
| Data_Efetivado_Estoque | datetime | SIM | |
| Data_Efetivado_Financeiro | datetime | SIM | |
| Data_Entrega | datetime | SIM | |
| Data_Imprimiu_Ticket | date | SIM | |
| Data_Passou_Desefetivacao_Estoque | datetime | SIM | |
| Data_Passou_Efetivacao_Estoque | datetime | SIM | |
| Data_Previsao_Recebimento | datetime | SIM | |
| Data_Sincronizacao | datetime | SIM | |

### Campos de Valores

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Acrescimo_Total_Geral | money | NÃO | |
| Acrescimo_Total_Prod | money | NÃO | |
| Acrescimo_Total_Serv | money | NÃO | |
| Acrescimo_Valor_Somado | money | NÃO | |
| Cubagem_Final | money | NÃO | |
| Desconto_Total_Geral | money | NÃO | |
| Desconto_Total_Prod | money | NÃO | |
| Desconto_Total_Serv | money | NÃO | |
| Desconto_Valor_Somado | money | NÃO | |
| Despesa_Acessoria | money | NÃO | |
| Frete_Valor_Separado_Somado | money | NÃO | |
| Frete_Valor_Somado | money | NÃO | |
| II_Base_Somado | money | NÃO | |
| II_Valor_Somado | money | NÃO | |
| Peso_Final | money | NÃO | |
| Peso_Liquido_Final | money | NÃO | |
| Preco_Custo_Medio_Somado | money | NÃO | |
| Preco_Custo_Medio_Somado | money | NÃO | |
| Preco_Custo_Somado | money | NÃO | |
| Preco_Custo_Somado | money | NÃO | |
| Preco_Final_Somado | money | NÃO | |
| Preco_Final_Somado | money | NÃO | |
| Preco_Total_Com_Desconto_Somado | money | NÃO | |
| Preco_Total_Com_Desconto_Somado | money | NÃO | |
| Preco_Total_Prod | money | NÃO | |
| Preco_Total_Prod | money | NÃO | |
| Preco_Total_Prod_Sem_Desconto_Somado | money | NÃO | |
| Preco_Total_Prod_Sem_Desconto_Somado | money | NÃO | |
| Preco_Total_Sem_Desconto_Somado | money | NÃO | |
| Preco_Total_Sem_Desconto_Somado | money | NÃO | |
| Preco_Total_Serv | money | NÃO | |
| Preco_Total_Serv | money | NÃO | |
| Preco_Total_Serv_Sem_Desconto_Somado | money | NÃO | |
| Preco_Total_Serv_Sem_Desconto_Somado | money | NÃO | |
| Qtde_Total_Geral | money | NÃO | |
| Qtde_Total_Prod | money | NÃO | |
| Qtde_Total_Serv | money | NÃO | |
| Valor_Container | money | SIM | |
| Valor_Pagamento_Ecommerce_Integracoes | money | SIM | |

### Campos de Impostos

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
| Apagado | bit | NÃO | |
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
| Criado_Por | varchar | NÃO | |
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
| Hora_Imprimiu_Ticket | time | SIM | |
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
| Sequencia | int | NÃO | |
| Situacao_Expedicao | varchar | NÃO | |
| Situacao_Liberacao_Permissoes | varchar | SIM | |
| Situacao_VEF | varchar | NÃO | |
| Tipo_Operacao | varchar | NÃO | |
| Tipo_Recebimento_Frete | varchar | NÃO | |
| Versao_Efetivacao_Estoque | varchar | NÃO | |
| Versao_Efetivacao_Financeiro | varchar | NÃO | |
| Versao_Gravacao | varchar | NÃO | |



