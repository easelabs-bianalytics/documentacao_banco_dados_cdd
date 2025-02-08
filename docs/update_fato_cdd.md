# Atualização da Tabela Fato CDDD 

## Introdução
Este documento detalha o processo de **ETL (Extração, Transformação e Carga)** aplicado ao schema `CDDD` e sua **tabela fato**. O processo abrange a **extração de dados**, a **aplicação de transformações** e a **inserção dos dados** processados no banco de dados de destino.

## Objetivo
O objetivo é manter a integridade dos dados e garantir que a tabela fato seja atualizada no banco de dados de destino diariamente, aplicando as transformações necessárias para a consistência e a formatação correta.

## Arquivos e Dependências
O processo depende de alguns arquivos de configuração e módulos auxiliares:

- Módulos de funções:

    - **`configurar_engine`**: Configuração das conexões com o banco de dados.

    - **`carregar_dados`**: Função para carregar os dados das tabelas.

    - **`gerar_hash`**: Função para gerar um hash único para cada linha de dados.

    - **`inserir_dados_com_tratamento`**: Função para inserir os dados tratados no banco de dados de destino.

## Estrutura de Dados
A tabela envolvida neste processo é:

- **Tabela `FATO` no esquema `cddd`**: Contém dados principais de unidades e identificadores de registros.

### Processo de ETL

#### 1. Carregar Dados
A função começa carregando os dados do schemas `cddd`. 

#### 2. Aplicar Transformações
As transformações aplicadas nos dados incluem:

- Conversão de colunas de data para o formato `YYYY-MM`.

- Renomeação de colunas financeiras (de `r$` para `valor_`). Essa renomeação é feita porque o Postgresql não aceita o $ como nome de coluna.

- Renomeação da coluna **`cod_anomesdia`** para **`cod_anomes`**. Essa alteração é necessária porque a tabela já carregada no banco de dados se origina do **CDD-Mensal**, que utiliza essa nomenclatura. A padronização evita inconsistências na inserção de dados devido a diferenças nos nomes das colunas.

- Conversão de valores financeiros e numéricos para o formato adequado.

#### 3. Gerar Hash
Após as transformações, um hash único é gerado para cada linha de dados para garantir a unicidade e integridade dos registros.


#### 4 . Inserir Dados no Banco de Dados
Por fim, os dados tratados são inseridos no banco de dados de destino, utilizando a função `inserir_dados_com_tratamento`.

## Conclusão
O processo de ETL descrito neste documento garante a atualização e integridade dos dados nas tabela `FATO` do schema `cddd`. Ele realiza transformações nos dados, gera hashes para garantir a unicidade dos registros e insere os dados no banco de dados de destino.

Esse fluxo garante que a tabela de fato seja mantida atualizada e que as transformações necessárias sejam aplicadas de forma consistente.
