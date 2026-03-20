# Esquema Tabela dbo.Movimento_Prod_Serv

Contém os dados individuais dos itens de cada movimento.

### Relacionamentos Críticos:

dbo.Movimento 1:N dbo.Movimento_Prod_Serv

→ Uma venda pode possuir vários itens

dbo.Prod_Serv 1:N dbo.Movimento_Prod_Serv

→ Um produto pode aparecer em vários itens de vendas

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

As Colunas com nome "Ordem_*" são chaves estrangeiras.


| Coluna | Tipo de Dados | Permite Nulo | Tabela Origem |
|--------|---------------|--------------|---------------|
| Ordem (PK) | int | NÃO | |
| Ordem_CEST | int | SIM | dbo.CEST |
| Ordem_CFOP_Prod_NF | int | NÃO | dbo.CFOP |
| Ordem_CFOP_Serv_NF | int | NÃO | dbo.CFOP |
| Ordem_Classe_Imposto | int | NÃO | dbo.Classe_Imposto |
| Ordem_Comissionado | int | NÃO | dbo.Cli_For |
| Ordem_Cor | int | NÃO | dbo.Cores |
| Ordem_Edicao | int | NÃO | dbo.Prod_Serv_Edicao |
| Ordem_Enquadramento_IPI | int | SIM | dbo.Enquadramento_IPI |
| Ordem_Filial_Estoque | int | SIM | dbo.Filiais |
| Ordem_Funcionario_Alteracao | int | NÃO | dbo.Funcionarios |
| Ordem_Funcionario_Digitado_Por | int | SIM | dbo.Funcionarios |
| Ordem_Grupo_CC_CF | int | SIM | ? |
| Ordem_Grupo_CC_NF | int | SIM | ? |
| Ordem_Lote | int | NÃO | dbo.Prod_Serv_Lote |
| Ordem_Motivo_Desconto | int | SIM | dbo.Motivo_Desconto |
| Ordem_Motivo_Devolucao | int | SIM | dbo.Motivo_Devolucao |
| Ordem_Motivo_Promocao | int | SIM | dbo.Motivo_Desconto |
| Ordem_Movimento | int | NÃO | dbo.Movimento |
| Ordem_Movimento_Prod_Serv_Importado | int | SIM | ? |
| Ordem_NBS | int | NÃO | dbo.NBS |
| Ordem_NCM | int | NÃO | dbo.NCM |
| Ordem_Prod_Serv | int | NÃO | dbo.Prod_Serv |
| Ordem_Tabela_PMC | int | NÃO | dbo.Tabelas_Preco |
| Ordem_Tabela_Preco | int | NÃO | dbo.Tabelas_Preco |
| Ordem_Tabela_Preco_Custo | int | NÃO | dbo.Tabelas_Preco |
| Ordem_Tabela_Preco_Custo_Medio | int | NÃO | dbo.Tabelas_Preco |
| Ordem_Tamanho | int | NÃO | dbo.Tamanhos |
| Ordem_Tecnico1 | int | NÃO | dbo.Funcionarios |
| Ordem_Tecnico2 | int | NÃO | dbo.Funcionarios |
| Ordem_Vendedor | int | NÃO | dbo.Funcionarios |
| Ordem_Vendedor2 | int | NÃO | dbo.Funcionarios |
### Campos de Data

#### Regra para Datas
Os campo de data devem sempre ser convertido em DD/MM/AAAA, utilizando a função CONVERT(VARCHAR(10),Campo_de_Data,103).

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Data_Alteracao | datetime | NÃO | |
| Data_Desefetivacao_Estoque | datetime | SIM | |
| Data_Efetivacao_Estoque | datetime | SIM | |
| Data_Passou_Desefetivacao_Estoque | datetime | SIM | |
| Data_Passou_Efetivacao_Estoque | datetime | SIM | |
| Data_Servico_Concluido | datetime | SIM | |
| Periodo_Desconto_Data_Final | date | SIM | |
| Periodo_Desconto_Data_Inicial | date | SIM | |

### Campos de Valores

Os campos de valores da tabela dbo.Movimento sempre serão a soma de todas as mercadorias da movimentação. Se refere as totalizações da movimentação.

Deve ser respeitado a Regra para Campos com Três Versões.

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Acrescimo_Percentual | money | NÃO | |
| Acrescimo_Valor | money | NÃO | |
| Comissao_Comissionado | money | NÃO | |
| Comissao_Tecnico1 | money | NÃO | |
| Comissao_Tecnico2 | money | NÃO | |
| Comissao_Vendedor1 | money | NÃO | |
| Comissao_Vendedor2 | money | NÃO | |
| Cubagem | money | NÃO | |
| Custo_Adicional | money | SIM | |
| Custo_Adicional_Valor | money | SIM | |
| Desconto_Percentual | money | NÃO | |
| Desconto_Percentual_Calculado | money | NÃO | |
| Desconto_Percentual_Promocoes_Especiais | money | SIM | |
| Desconto_Valor | money | NÃO | |
| Desconto_Valor_Promocoes_Especiais | money | SIM | |
| Desp_Aduaneira_II | money | NÃO | |
| Despesas_Valor | money | NÃO | |
| Frete_Valor | money | NÃO | |
| Frete_Valor_Separado | money | NÃO | |
| Margem_Flex_Valor | money | SIM | |
| Peso_Bruto | money | NÃO | |
| Peso_Liquido | money | SIM | |
| Preco_Custo | money | NÃO | |
| Preco_Custo_Mantido_Orcamento | bit | SIM | |
| Preco_Custo_Medio | money | NÃO | |
| Preco_Final | money | NÃO | |
| Preco_Final_Relatorio | money | SIM | |
| Preco_Final_Sem_Frete | money | SIM | |
| Preco_PMC | money | NÃO | |
| Preco_Por_Caixa | money | NÃO | |
| Preco_Total_Com_Desconto | money | NÃO | |
| Preco_Total_Sem_Desconto | money | NÃO | |
| Preco_Unitario | money | NÃO | |
| Preco_Unitario_Original | money | SIM | |
| Quantidade | money | NÃO | |
| Quantidade_Caixas_Com | money | NÃO | |
| Quantidade_Itens | money | NÃO | |

### Campos de Impostos

Valores individuais de cada imposto da movimentação. 

Esses valores já estão inclusos no campo Preco_Final_Somado e não precisar ser somados à ele novamente.

Deve ser respeitado a Regra para Campos com Três Versões.

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
Base_BFCP | money | SIM |
Base_II | money | NÃO |
Carga_Tributaria | money | NÃO |
Carga_Tributaria_Chave | varchar | NÃO |
Carga_Tributaria_Estadual_Perc | money | NÃO |
Carga_Tributaria_Estadual_Valor | money | NÃO |
Carga_Tributaria_Federal_Perc | money | NÃO |
Carga_Tributaria_Federal_Valor | money | NÃO |
Carga_Tributaria_Fonte | varchar | NÃO |
Carga_Tributaria_Fonte2 | varchar | NÃO |
Carga_Tributaria_Municipal_Perc | money | NÃO |
Carga_Tributaria_Municipal_Valor | money | NÃO |
Carga_Tributaria_Percentual | money | NÃO |
CBS_Aliquota_Efetiva_Percentual | money | SIM |
CBS_Aliquota_Efetiva_TribRegular_Percentual | money | SIM |
CBS_Base | money | SIM |
CBS_Calcula_CredPres | bit | SIM |
CBS_CredPres_Classif | varchar | SIM |
CBS_CredPres_CondSus_Valor | money | SIM |
CBS_CredPres_Percentual | money | SIM |
CBS_CredPres_Valor | money | SIM |
CBS_Devolucao_Tributos_Valor | money | SIM |
CBS_Diferido_Percentual | money | SIM |
CBS_Diferido_Valor | money | SIM |
CBS_Monofasico_Aliquota_AdRem | money | SIM |
CBS_Monofasico_Aliquota_AdRem_Retencao | money | SIM |
CBS_Monofasico_Aliquota_AdRem_Retido | money | SIM |
CBS_Monofasico_Diferido_Percentual | money | SIM |
CBS_Monofasico_Diferido_Valor | money | SIM |
CBS_Monofasico_Retencao_Valor | money | SIM |
CBS_Monofasico_Retido_Valor | money | SIM |
CBS_Monofasico_Total | money | SIM |
CBS_Monofasico_Valor | money | SIM |
CBS_Percentual | money | SIM |
CBS_Reducao_Aliquota_Percentual | money | SIM |
CBS_TransfCred_Valor | money | SIM |
CBS_TribCompraGov_Percentual | money | SIM |
CBS_TribCompraGov_Valor | money | SIM |
CBS_TribRegular_Valor | money | SIM |
CBS_Valor | money | SIM |
CEST | varchar | SIM |
CFOP_CF | varchar | NÃO |
CFOP_NF | varchar | NÃO |
CFOP_Prod_Serv_Composto | bit | SIM |
Cod_Classifica_Serv | varchar | SIM |
Codigo_Beneficio_Fiscal | varchar | SIM |
Codigo_Tributacao_ISS | varchar | SIM |
COFINS_Calcula_Subs | bit | NÃO |
COFINS_Normal_Base | money | NÃO |
COFINS_Normal_Percentual | money | NÃO |
COFINS_Normal_Valor | money | NÃO |
COFINS_Subst_Base | money | NÃO |
COFINS_Subst_Percentual | money | NÃO |
COFINS_Subst_Soma_Total | bit | SIM |
COFINS_Subst_Valor | money | NÃO |
Converteu_Em_CST_60_XML | bit | SIM |
FCI | varchar | NÃO |
FCI_Perc_Importado | money | NÃO |
FCP_Normal_Base | money | SIM |
FCP_Normal_Percentual | money | SIM |
FCP_Normal_Valor | money | SIM |
FCP_Retido_Base | money | SIM |
FCP_Retido_Percentual | money | SIM |
FCP_Retido_Valor | money | SIM |
FCP_ST_Base | money | SIM |
FCP_ST_Percentual | money | SIM |
FCP_ST_Valor | money | SIM |
IBS_Calcula_CredPres | bit | SIM |
IBS_CredPresZFM_Tipo | varchar | SIM |
IBS_CredPresZFM_Valor | money | SIM |
IBS_Est_Aliquota_Efetiva_Percentual | money | SIM |
IBS_Est_Aliquota_Efetiva_TribRegular_Percentual | money | SIM |
IBS_Est_Base | money | SIM |
IBS_Est_Devolucao_Tributos_Valor | money | SIM |
IBS_Est_Diferido_Percentual | money | SIM |
IBS_Est_Diferido_Valor | money | SIM |
IBS_Est_Percentual | money | SIM |
IBS_Est_Reducao_Aliquota_Percentual | money | SIM |
IBS_Est_TribCompraGov_Percentual | money | SIM |
IBS_Est_TribCompraGov_Valor | money | SIM |
IBS_Est_TribRegular_Valor | money | SIM |
IBS_Est_Valor | money | SIM |
IBS_Monofasico_Aliquota_AdRem | money | SIM |
IBS_Monofasico_Aliquota_AdRem_Retencao | money | SIM |
IBS_Monofasico_Aliquota_AdRem_Retido | money | SIM |
IBS_Monofasico_Diferido_Percentual | money | SIM |
IBS_Monofasico_Diferido_Valor | money | SIM |
IBS_Monofasico_Retencao_Valor | money | SIM |
IBS_Monofasico_Retido_Valor | money | SIM |
IBS_Monofasico_Total | money | SIM |
IBS_Monofasico_Valor | money | SIM |
IBS_Mun_Aliquota_Efetiva_Percentual | money | SIM |
IBS_Mun_Aliquota_Efetiva_TribRegular_Percentual | money | SIM |
IBS_Mun_Base | money | SIM |
IBS_Mun_CredPres_Classif | varchar | SIM |
IBS_Mun_CredPres_CondSus_Valor | money | SIM |
IBS_Mun_CredPres_Percentual | money | SIM |
IBS_Mun_CredPres_Valor | money | SIM |
IBS_Mun_Devolucao_Tributos_Valor | money | SIM |
IBS_Mun_Diferido_Percentual | money | SIM |
IBS_Mun_Diferido_Valor | money | SIM |
IBS_Mun_Percentual | money | SIM |
IBS_Mun_Reducao_Aliquota_Percentual | money | SIM |
IBS_Mun_TribCompraGov_Percentual | money | SIM |
IBS_Mun_TribCompraGov_Valor | money | SIM |
IBS_Mun_TribRegular_Valor | money | SIM |
IBS_Mun_Valor | money | SIM |
IBS_TransfCred_Valor | money | SIM |
IBS_Valor | money | SIM |
IBSCBS_Base_Credito_Presumido | money | SIM |
IBSCBS_Calcula_CredPres | bit | SIM |
IBSCBS_Calcula_Diferimento | bit | SIM |
IBSCBS_Calcula_Reducao | bit | SIM |
IBSCBS_Calcula_TribReg | bit | SIM |
IBSCBS_ClassTrib | varchar | SIM |
IBSCBS_ClassTrib_TribRegular | varchar | SIM |
IBSCBS_CST | varchar | SIM |
IBSCBS_CST_TribRegular | varchar | SIM |
IBSCBS_IVA_Dual | money | SIM |
IBSCBS_IVA_Dual_Valor | money | SIM |
IBSCBS_Monofasico_Base | money | SIM |
IBSCBS_Monofasico_Retencao_Base | money | SIM |
IBSCBS_Monofasico_Retido_Base | money | SIM |
ICMS_Carga_Media_Percentual | money | NÃO |
ICMS_CST_CSOSN | varchar | NÃO |
ICMS_Desonerado_Valor | money | NÃO |
ICMS_Diferido_Base | money | NÃO |
ICMS_Diferido_Modalidade_Base | int | NÃO |
ICMS_Diferido_Normal_Percentual | money | NÃO |
ICMS_Diferido_Normal_Valor | money | NÃO |
ICMS_Diferido_Percentual | money | NÃO |
ICMS_Diferido_Valor_Diferimento | money | NÃO |
ICMS_Diferido_Valor_Operacao | money | NÃO |
ICMS_Efetivo_Base | money | SIM |
ICMS_Efetivo_Percentual | money | SIM |
ICMS_Efetivo_Valor | money | SIM |
ICMS_FCP_Percentual | money | SIM |
ICMS_FCP_Valor | money | SIM |
ICMS_Monofasico_Diferido_Percentual_Diferimento | money | SIM |
ICMS_Monofasico_Diferido_Valor | money | SIM |
ICMS_Monofasico_Diferido_Valor_ICMS_Devido | money | SIM |
ICMS_Monofasico_Normal_Aliquota_AdRem | money | SIM |
ICMS_Monofasico_Normal_Base | money | SIM |
ICMS_Monofasico_Normal_Valor | money | SIM |
ICMS_Monofasico_Reducao_Aliquota_AdRem | money | SIM |
ICMS_Monofasico_Reducao_Motivo | varchar | SIM |
ICMS_Monofasico_Retencao_Aliquota_AdRem | money | SIM |
ICMS_Monofasico_Retencao_Base | money | SIM |
ICMS_Monofasico_Retencao_Valor | money | SIM |
ICMS_Monofasico_Retido_Aliquota_AdRem | money | SIM |
ICMS_Monofasico_Retido_Base | money | SIM |
ICMS_Monofasico_Retido_Valor | money | SIM |
ICMS_Motivo_Desoneracao | varchar | NÃO |
ICMS_Norm_Omitir_Opcionais_51 | bit | SIM |
ICMS_Normal_Base | money | NÃO |
ICMS_Normal_Base_Sem_Reducao | money | NÃO |
ICMS_Normal_Codigo_Beneficio_Reducao | varchar | SIM |
ICMS_Normal_Modalidade_Base | int | NÃO |
ICMS_Normal_Perc_Reducao | money | NÃO |
ICMS_Normal_Percentual | money | NÃO |
ICMS_Normal_Percentual_Desoneracao | money | SIM |
ICMS_Normal_Percentual_Efetivo_SAT | money | SIM |
ICMS_Normal_Soma_Despesas | bit | SIM |
ICMS_Normal_Soma_Frete | bit | SIM |
ICMS_Normal_Soma_IPI | bit | SIM |
ICMS_Normal_Subtrair_Valor_Desonerado_Total_Nota | bit | SIM |
ICMS_Normal_Valor | money | NÃO |
ICMS_Partilhado_Base | money | SIM |
ICMS_Partilhado_Perc_Reducao | money | SIM |
ICMS_Partilhado_Percentual | money | SIM |
ICMS_Partilhado_Percentual_Interestadual | money | SIM |
ICMS_Partilhado_Percentual_Partilha | money | SIM |
ICMS_Partilhado_Valor_Destino | money | SIM |
ICMS_Partilhado_Valor_Remetente | money | SIM |
ICMS_Reducao_Efetivo_Percentual | money | SIM |
ICMS_Retido_Base | money | NÃO |
ICMS_Retido_Modalidade_Base | int | NÃO |
ICMS_Retido_Percentual | money | NÃO |
ICMS_Retido_Valor | money | NÃO |
ICMS_Simples_Base | money | NÃO |
ICMS_Simples_Percentual | money | NÃO |
ICMS_Simples_Valor | money | NÃO |
ICMS_Subst_Base | money | NÃO |
ICMS_Subst_Base_Sem_Reducao | money | NÃO |
ICMS_Subst_Modalidade_Base | int | NÃO |
ICMS_Subst_Perc_Reducao | money | NÃO |
ICMS_Subst_Percentual | money | NÃO |
ICMS_Subst_Percentual_MVA | money | NÃO |
ICMS_Subst_Valor | money | NÃO |
ICMS_Substituto_Valor | money | SIM |
ICMSST_Repasse_Base | money | SIM |
ICMSST_Repasse_Calcular | bit | SIM |
ICMSST_Repasse_Percentual | money | SIM |
ICMSST_Repasse_Valor | money | SIM |
INSS_Base | money | SIM |
INSS_Percentual | money | SIM |
INSS_Valor | money | SIM |
Invalido | bit | NÃO |
IOF_II | money | NÃO |
IPI_Base | money | NÃO |
IPI_CST | varchar | NÃO |
IPI_Manualmente_Entrada | bit | SIM |
IPI_Percentual | money | NÃO |
IPI_Valor | money | NÃO |
IPI_Valor_Devolvido | money | NÃO |
IS_Base | money | SIM |
IS_ClassTrib | varchar | SIM |
IS_CST | varchar | SIM |
IS_Percentual | money | SIM |
IS_Percentual_Especial | money | SIM |
IS_Quantidade_Tributavel | money | SIM |
IS_Unidade_Medida_Tributavel | varchar | SIM |
IS_Valor | money | SIM |
ISS_Base | money | NÃO |
ISS_Indicador_Exigibilidade | int | NÃO |
ISS_Indicador_Incentivo_Fiscal | int | NÃO |
ISS_Percentual | money | NÃO |
ISS_Percentual_Deducao | money | SIM |
ISS_Valor | money | NÃO |
ISS_Valor_Sem_Arredondar | money | SIM |
ISS_Valor_Subtrair_Total | money | SIM |
Natureza_Operacao_ISSQN | int | SIM |
NBS | varchar | NÃO |
NCM | varchar | NÃO |
Numero_RECOPI | varchar | NÃO |
Origem_Mercadoria |  |  |
Perc_BFCP | money | SIM |
Percentual_Difal | money | SIM |
PIS_Calcula_Subs | bit | NÃO |
PIS_COFINS_CST | varchar | NÃO |
PIS_COFINS_Natureza_Base_Credito | varchar | SIM |
PIS_Normal_Base | money | NÃO |
PIS_Normal_Percentual | money | NÃO |
PIS_Normal_Valor | money | NÃO |
PIS_Subst_Base | money | NÃO |
PIS_Subst_Percentual | money | NÃO |
PIS_Subst_Soma_Total | bit | SIM |
PIS_Subst_Valor | money | NÃO |
Retencao_Calcular | bit | SIM |
Retencao_Calcular_Produtos_Com_Desconto | bit | SIM |
Retencao_Produto_COFINS_Base | money | SIM |
Retencao_Produto_COFINS_Percentual | money | SIM |
Retencao_Produto_COFINS_Valor | money | SIM |
Retencao_Produto_CSLL_Base | money | SIM |
Retencao_Produto_CSLL_Percentual | money | SIM |
Retencao_Produto_CSLL_Valor | money | SIM |
Retencao_Produto_IR_Base | money | SIM |
Retencao_Produto_IR_Percentual | money | SIM |
Retencao_Produto_IR_Valor | money | SIM |
Retencao_Produto_IR_Valor_Minimo | money | SIM |
Retencao_Produto_PIS_Base | money | SIM |
Retencao_Produto_PIS_COFINS_CSLL_Valor_Minimo | money | SIM |
Retencao_Produto_PIS_Percentual | money | SIM |
Retencao_Produto_PIS_Valor | money | SIM |
Valor_BFCP | money | SIM |
Valor_Deducao_Configurado | money | SIM |
Valor_II | money | NÃO |
Valor_II_Soma_Total | bit | NÃO |
Valores_Antes_Converter_CTS60 | varchar | SIM |

### Campos Gerais

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Agrupador | varchar | SIM | |
| Codigo_Enquadramento_IPI | int | SIM | |
| Codigo_Motivo_Desconto | varchar | SIM | |
| Codigo_Motivo_Promocao | varchar | SIM | |
| Codigo_Prod_Serv_Importado_Xml | varchar | SIM | |
| Complemento | varchar | NÃO | |
| Complemento_Extra | varchar | SIM | |
| Conferencia_Inserido_Posteriomente | bit | SIM | |
| Conferencia_Qtde_Conferida | money | SIM | |
| CRM_BONUS | bit | SIM | |
| Devolucao_IPI_Soma_ICMS_Normal | bit | SIM | |
| Devolucao_IPI_Soma_ICMS_ST | bit | SIM | |
| Devolucao_Simplificada_Nota_Vinculada | varchar | SIM | |
| Devolucao_Simplificada_Numero_Linha_Considerar | int | SIM | |
| Devolucao_Simplificada_Numero_Linha_Vinculado | int | SIM | |
| Devolucao_Simplificada_Ordem_Movimento_Prod_Serv_Considerar | int | SIM | |
| Devolucao_Simplificada_Sequencia_Considerar | int | SIM | |
| Devolucao_Simplificada_Sequencia_Vinculada | int | SIM | |
| Estoque_Desefetivado | bit | NÃO | |
| Estoque_Efetivado | bit | NÃO | |
| Estoque_Efetivado_OS | bit | NÃO | |
| Ex_Tipi | varchar | NÃO | |
| Linha_Excluida | bit | NÃO | |
| Medicamento | bit | NÃO | |
| Numero_Item_Pedido | int | NÃO | |
| Numero_Item_Pedido_V2 | varchar | SIM | |
| Numero_Linha | int | NÃO | |
| Numero_Linha_Pai | int | SIM | |
| Numero_Ordem_Servico | int | NÃO | |
| Observacao | varchar | NÃO | |
| Pedido | varchar | NÃO | |
| Percentual_Devolvido | money | NÃO | |
| Periodo_Desconto_Percentual | money | SIM | |
| Periodo_Desconto_Possui_Limite | bit | SIM | |
| Prod_Serv_Controla_Estoque | bit | NÃO | |
| Servico_Concluido | bit | NÃO | |
| Status_Remessa | int | SIM | |
| Tipo_Retirada | varchar | SIM | |
| Tipo_Tributacao_Servico | int | SIM | |
| Totalizador_Parcial | varchar | NÃO | |
| Utilizou_Codigo_Promocional | bit | SIM | |
| Utilizou_Comissao_Por_Filial | bit | SIM | |
| Utilizou_Preco_Promocional | bit | NÃO | |

### Campos com pouca recorrência

| Coluna | Tipo de Dados | Permite Nulo | Descrição |
|--------|---------------|--------------|-----------|
| Difal_Base_Modo_Calculo | varchar | SIM | |
| Diferenca_Cupom | money | NÃO | |
| Discriminacao_Servico | varchar | SIM | |
| Enviar_BFCP | bit | SIM | |
| Enviar_ICMSSubstituto | bit | SIM | |
| Enviar_ICMSSubstituto_Zerado | bit | SIM | |
| Etiqueta | int | NÃO | |
| Excluido_Pelo_Shop | bit | NÃO | |
| Identificador_Kit | int | SIM | |
| Identificadores_Promocao_DS | varchar | SIM | |
| Imp_Fiscal_Base | money | NÃO | |
| Imp_Fiscal_Percentual | money | NÃO | |
| Imp_Fiscal_Tipo | varchar | NÃO | |
| Imp_Fiscal_Valor | money | NÃO | |
| Impostos_Configurados | bit | SIM | |
| JSON_Lista_Etapas_Calculos_Personalizados | varchar | SIM | |
| Largura_Digitada | money | SIM | |
| Luxottica_Cliente_Deixou_Aro_Uso | bit | SIM | |
| Pedido_Garantia | bit | SIM | |
| Reservado_Loja | bit | SIM | |
| Valida_R05 | varchar | NÃO | |
| Valida_R05V2 | varchar | SIM | |
| Validacao | varchar | NÃO | |