# Atualização da Tabela Fato TD

## Introdução
Este documento detalha o processo de **ETL (Extração, Transformação e Carga)** aplicado ao schema `TD` e sua **tabela fato**. O processo abrange a **extração de dados**, a **aplicação de transformações** e a **inserção dos dados** processados no banco de dados de destino.

## Objetivo
O objetivo é manter a integridade dos dados e garantir que a tabela fato seja atualizada no banco de dados de destino diariamente, aplicando as transformações necessárias para a consistência e a formatação correta.

## Arquivos e Dependências
O processo depende de alguns arquivos de configuração e módulos auxiliares:

- **`ultima_data_fato_td.txt`**: Contém a última data de fato TD processada.

- Módulos de funções:

    - **`configurar_engine`**: Configuração das conexões com o banco de dados.

    - **`carregar_dados`**: Função para carregar os dados das tabelas.

    - **`gerar_hash`**: Função para gerar um hash único para cada linha de dados.

    - **`inserir_dados_com_tratamento`**: Função para inserir os dados tratados no banco de dados de destino.

    - **`execucao_no_periodo_permitido`**: Função para verificar se o script está dentro do período de execução.


#### Parâmetros:
- **`DATA_FILE`**: Caminho do arquivo que contém a última data processada. O padrão é 'ultima_data_fato_td.txt'.

### Processo de ETL

#### Verificação de período

A função verifica se a data atual está entre os dias 15 e o último dia do mês. Caso a condição seja atendida, o script prossegue com a execução; caso contrário, ele é interrompido.  

Obs: Essa lógica evita consultas desnecessárias ao banco de dados Snowflake, uma vez que a tabela TD é atualizada apenas uma vez por mês, entre os dias 15 e 30/31. Esse intervalo pode ser reduzido se tivermos uma data exata de atualização da tabela, otimizando ainda mais o número de consultas realizadas.

#### 1. Carregar Dados
A função começa carregando os dados do schema `td`. A função carrega apenas os dados mais recentes, com base na última data salva no arquivo.

#### 2. Aplicar Transformações
As transformações aplicadas nos dados incluem:
- Conversão de colunas de data para o formato `YYYY-MM`.
- Renomeação de colunas financeiras (de `r$` para `valor_`).
- Conversão de valores financeiros e numéricos para o formato adequado.

#### 3. Gerar Hash
Após as transformações, um hash único é gerado para cada linha de dados para garantir a unicidade e integridade dos registros.

#### 4. Atualizar Última Data
A última data processada é salva no arquivo `ultima_data_fato_td.txt`, garantindo que, na próxima execução, apenas novos dados sejam carregados.

#### 5. Inserir Dados no Banco de Dados
Por fim, os dados tratados são inseridos no banco de dados de destino, utilizando a função `inserir_dados_com_tratamento`.

## Conclusão
O processo de ETL descrito neste documento garante a atualização e integridade dos dados nas tabela `FATO` do schema `td`. Ele realiza transformações nos dados, gera hashes para garantir a unicidade dos registros e insere os dados no banco de dados de destino.

Esse fluxo garante que a tabela de fato seja mantida atualizada e que as transformações necessárias sejam aplicadas de forma consistente.
