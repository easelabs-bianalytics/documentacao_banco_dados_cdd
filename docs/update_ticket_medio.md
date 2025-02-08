# Atualização de Dados de Ticket Médio

Este script realiza o processo de atualização dos dados de "Ticket Médio" a partir de arquivos armazenados no SharePoint. O processo envolve a extração, transformação e armazenamento das informações em uma tabela do banco de dados de destino.

## Funções Implementadas

### 1. `clean_column_name(name)`
Função que limpa os nomes das colunas, removendo acentos, convertendo para minúsculas e substituindo espaços e pontos por underscores.

#### Parâmetros
- `name`: Nome da coluna a ser limpo.

#### Retorno
- Retorna o nome da coluna limpo.

### 2. `atualizar_ticket_medio()`
Função principal que executa o processo de atualização dos dados de Ticket Médio:

- **Baixar Arquivos**: Baixa todos os arquivos Excel armazenados no SharePoint para o diretório local configurado.

- **Processamento de Arquivos**: Após o download, os arquivos são processados para extrair e limpar os dados de Ticket Médio.

- **Transformações nos Dados**: A função realiza várias transformações e limpezas nos dados.

- **Armazenamento no Banco**: Após o processamento, os dados são armazenados na tabela `fato_ticket_medio` no banco de dados de destino.

#### Subprocessos
1. **Baixar Arquivos**: O script baixa todos os arquivos Excel da pasta remota configurada no SharePoint para um diretório local.
2. **Processamento de Dados**:
      - Os arquivos Excel são lidos, e as colunas são ajustadas com base na 6ª linha de cada arquivo.

      - As linhas com dados ausentes na coluna `Dt Emissão` são removidas.

      - Algumas colunas indesejadas são removidas, e o nome da coluna `Name` é alterado para `Nome da Origem`.

      - As colunas `MODAL` e `SKU` são derivadas a partir de outras colunas usando as funções `determinar_modal` e `determinar_produto`.
         
      - A coluna `Dt Emissão` é convertida para o formato de data, e uma coluna de período mensal é criada.

3. **Filtragem de Dados**: São filtrados apenas os dados relevantes, incluindo condições específicas para as colunas `Utilização`, `Status`, `UN`, e `Preço`.
4. **Cálculo de Ticket Médio**: O ticket médio é calculado por agrupamento de diversas colunas, e o valor é arredondado para duas casas decimais.
5. **Criação de Tabela Pivô**: Uma tabela pivô é criada para reorganizar os dados por Mês, COD_APRESENTACAO, SKU e Modal, com a média do ticket médio.
6. **Limpeza de Nomes de Colunas**: Os nomes das colunas na tabela pivô são limpos utilizando a função `clean_column_name()`.
7. **Armazenamento no Banco de Dados**: A tabela pivô final é armazenada no banco de dados de destino na tabela `fato_ticket_medio`.

### 3. **Funções Auxiliares**
- **`determinar_modal`**: Determina o modal com base nas informações da coluna `Entidade`.
- **`determinar_produto`**: Determina o SKU do produto com base nas informações da coluna `Nome`.
- **`drop_views`**: (Comentada no código) Função que pode ser usada para descartar views no banco de dados, se necessário.

## Transformações Realizadas nos Dados

- **Limpeza de Nomes de Colunas**: Os nomes das colunas são normalizados para minúsculas, acentos são removidos, e espaços e pontos são substituídos por underscores.
- **Remoção de Colunas Indesejadas**: Algumas colunas, como "ICMS Orig", "IPI CST", "PIS", entre outras, são removidas dos dados.
- **Cálculos de Ticket Médio**: O preço é agrupado por mês, COD_APRESENTACAO, SKU e MODAL, e o ticket médio é calculado como a média do preço.
- **Tabela Pivô**: A tabela pivô é criada para organizar os dados por Mês, COD_APRESENTACAO, SKU e Modal, com os valores de ticket médio.

## Dependências

O script depende das seguintes bibliotecas e módulos:
- `os`

- `pandas`

- `glob`

- `unidecode`

- `projeto_banco_cdd.modulos.sharepoint_downloader`

- `projeto_banco_cdd.modulos.functions_cdd`

### Observações

- O script baixa os arquivos Excel do SharePoint e processa apenas os arquivos que possuem a estrutura esperada.
- A função de cálculo de ticket médio usa a média do preço agrupado por Mês, COD_APRESENTACAO, SKU e MODAL.




