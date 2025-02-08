# Atualização de Extras, Acesso e Slow Changing Dimensions (SCD)

Este script realiza o processo de atualização dos dados de Extras, Acesso e Slow Changing Dimensions (SCD) no banco de dados a partir de arquivos armazenados no SharePoint. O processo envolve a extração, processamento e armazenamento das informações nas tabelas do banco de dados de destino.

## Funções Implementadas

### 1. `atualizar_extras_acesso_scd()`
Função principal que executa o processo de atualização dos dados de Extras, Acesso e SCD:
- **Baixar Arquivos**: A função baixa os arquivos Excel dos sites do SharePoint utilizando a classe `SharePointDownloader`.
- **Processamento de Arquivos**: Após o download, os arquivo são processados, limpando os dados e ajustando os tipos de colunas, como a conversão de datas.
- **Armazenamento no Banco**: Após o processamento, os dados são armazenados no banco de dados de destino utilizando a função `to_sql`.

#### Subprocessos
1. **Baixar e Processar Arquivos**: Arquivos de Extras, Acesso e SCD são baixados do SharePoint e processados.
2. **Limpeza de Dados**: A data de `Acesso` é convertida para o tipo `datetime` e registros com dados inválidos são removidos.
3. **Inserção de Dados**:
   - Dados de Extras e Acesso são inseridos nas tabelas `fato_extras` e `fato_acesso`, respectivamente.
   - As tabelas de dimensões SCD são inseridas no banco com os dados processados.

### 2. `baixar_e_processar()`
Função auxiliar para baixar e processar arquivos de maneira eficiente:
- Baixa o arquivo do SharePoint.
- Processa o arquivo Excel, convertendo os dados conforme necessário.

## Detalhamento das Transformações

- **Extras**: Os dados de extras são baixados e inseridos na tabela `fato_extras`.
- **Acesso**: A coluna `Data` da tabela de Acesso é convertida para o formato `datetime` e registros inválidos (com data inválida) são removidos.
- **SCD**: As dimensões do SCD são processadas, com a data de `data_fim_gerenciamento` sendo convertida para o tipo `datetime` quando presente. As dimensões são inseridas em tabelas correspondentes no banco de dados.

## Dependências

O script depende das seguintes bibliotecas:
- `os`
- `pandas`

Além disso, o script utiliza funções auxiliares do módulo `projeto_banco_cdd.modulos.sharepoint_downloader` para baixar os arquivos do SharePoint e do módulo `projeto_banco_cdd.modulos.functions_cdd` para processar e armazenar os dados.

## Observações

- Os arquivos de dados de Extras, Acesso e SCD são baixados de diferentes sites do SharePoint, conforme configurado no dicionário `site_url`.
- O banco de dados de destino é especificado através da variável `engine_destino`.
- As tabelas de dados são substituídas nas tabelas do banco de dados de destino, utilizando a opção `if_exists='replace'` para sobrescrever os dados existentes.

## Exemplos de Uso

```python
# Rodando o script
if __name__ == "__main__":
    atualizar_extras_acesso_scd()
