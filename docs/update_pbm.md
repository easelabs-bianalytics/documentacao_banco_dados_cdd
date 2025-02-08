# Atualização do PBM 

Este script realiza o processo de atualização dos dados do PBM (Product-Based Marketing) a partir de arquivos armazenados no SharePoint. O processo envolve a extração, transformação e armazenamento das informações nas tabelas do banco de dados de destino.

## Funções Implementadas

### 1. `atualizar_pbm()`
Função principal que executa o processo de atualização dos dados PBM:
- **Baixar Arquivos**: A função baixa todos os arquivos Excel da pasta correspondente no SharePoint utilizando a classe `SharePointDownloader`.
- **Processamento de Arquivos**: Após o download, os arquivos são processados para extrair os dados da folha de nome "TRANSAÇÃO".
- **Armazenamento no Banco**: Após o processamento, os dados são armazenados na tabela `fato_pbm` no banco de dados de destino.

#### Subprocessos
1. **Baixar Arquivos**: A função baixa todos os arquivos Excel armazenados no SharePoint para o diretório local configurado.
2. **Processamento de Dados**:
   - O script lê os arquivos Excel, verifica se a aba "TRANSAÇÃO" existe e carrega os dados da mesma.
   - Os dados da coluna `PRODUTO` são corrigidos, substituindo o nome de um produto específico.
   - As colunas `CNPJ PDV` e `EAN` têm os valores de string processados, removendo possíveis apóstrofos à esquerda.
3. **Armazenamento no Banco de Dados**: 
   - Os dados processados são concatenados em um único DataFrame e inseridos na tabela `fato_pbm` do banco de dados de destino.
   - A função usa o comando `to_sql()` para inserir os dados, substituindo qualquer dado anterior com `if_exists='replace'`.

### 2. **Baixar Arquivos do SharePoint**
A função `baixar_todos_arquivos()` da classe `SharePointDownloader` é utilizada para baixar todos os arquivos da pasta remota especificada.

### 3. **Processamento de Arquivos Excel**
Os arquivos Excel são carregados utilizando a biblioteca `pandas`. A função verifica se a folha "TRANSAÇÃO" existe nos arquivos e, em caso afirmativo, carrega esses dados. Caso contrário, o arquivo é ignorado.

## Transformações Realizadas nos Dados

- **Produto**: A coluna `PRODUTO` é corrigida para garantir que os nomes dos produtos sigam o formato correto (substituição de valores específicos).
- **CNPJ PDV e EAN**: A função remove caracteres indesejados (apóstrofos) das colunas `CNPJ PDV` e `EAN`.


## Dependências

O script depende das seguintes bibliotecas:

- `os`

- `glob`

- `pandas`

- `projeto_banco_cdd.modulos.sharepoint_downloader`

- `projeto_banco_cdd.modulos.functions_cdd`

### Observações

- O script busca por todos os arquivos Excel na pasta local de destino configurada e processa apenas os que contêm uma folha de nome "TRANSAÇÃO".
- O banco de dados de destino é especificado através da variável `engine_destino`.
- O processo de substituição na tabela do banco de dados usa a opção `if_exists='replace'`, o que garante que os dados existentes sejam substituídos.




