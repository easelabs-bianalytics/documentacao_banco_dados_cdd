# Atualização de Dimensões CDDD

Este script realiza o processo de ETL (Extração, Transformação e Carregamento) para atualizar as tabelas de dimensões (DIM) da schema CDD. O processo envolve a extração de dados das tabelas especificadas, aplicação de transformações necessárias e inserção dos dados tratados no banco de dados de destino.

## Tabelas Processadas
O script processa as seguintes tabelas dentro da schema `CDD`:

- FORCA_VENDAS

- PDVS

- CANAL

- INFORMANTES

- APRES

- FAB

- FV_DISTRITO

- FV_REGIONAL

- FV_TERRITORIO

- PROD

- TIPO_TX

- UTC

## Funções Implementadas

### 1. `atualizar_dim_cddd()`
Função principal que executa o processo de ETL:

- **Extração**: Carrega os dados das tabelas especificadas.
- **Transformação**:

  - Para a tabela `PDVS`, substitui valores nulos por uma data padrão e converte a coluna `data_abertura` para o formato `datetime`.

- **Carregamento**: Insere os dados tratados no banco de dados de destino.

### 2. `gerar_hash()`
Cria um hash único para cada linha de dados, utilizando as colunas especificadas.

### 3. `inserir_dados_com_tratamento()`
Insere os dados tratados nas tabelas do banco de dados de destino.

## Detalhamento das Transformações

- **PDVS**: A coluna `data_abertura` é preenchida com a data padrão `1900-01-01` nos casos de valores nulos, convertida para o formato `datetime` e as colunas `decada_abertura`, `data_consulta`, e `data_situacao` são removidas.
- **Hashing**: Para tabelas como `FORCA_VENDAS`, `PDVS`, `INFORMANTES`, `FV_DISTRITO`, `FV_REGIONAL`, `FV_TERRITORIO` e `UTC`, é gerado um hash único utilizando colunas específicas.

## Execução

A execução do processo é realizada ao rodar o script principal `atualizar_dim_cddd()`. Isso irá:

1. Processar os dados de todas as tabelas.
2. Aplicar as transformações necessárias.
3. Inserir os dados tratados no banco de dados de destino.

## Dependências

O script depende das seguintes bibliotecas:
- `pandas`
- `sqlalchemy`
- `snowflake.sqlalchemy`
- `datetime`

Além disso, o script também utiliza funções auxiliares importadas do módulo `projeto_banco_cdd.modulos.functions_cdd`.

## Observações

    - O script foi projetado para funcionar com a schema `CDD`, mas pode ser modificado para outros schemas se necessário.
    - As transformações podem ser ajustadas conforme a necessidade das tabelas processadas.
    - A função `atualizar_dim_cddd()` é a principal função de execução, chamada automaticamente ao rodar o script.


