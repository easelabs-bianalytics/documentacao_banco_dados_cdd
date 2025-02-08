# Estrutura da Pasta `dados`

A pasta `dados` é organizada para armazenar e gerenciar diferentes tipos de arquivos relacionados à execução de processos de transformação de dados e auditoria. Abaixo estão as subpastas que fazem parte dessa estrutura:

## Estrutura das Pastas

### 1. **data**
A pasta `data` contém informações relacionadas à última data de atualização dos dados na tabela fato `td`. Essa pasta é usada para controlar a data da última execução e garantir que os dados mais recentes sejam processados corretamente.

- **Função**: Guarda a última data dos dados da tabela fato.
- **Uso**: Utilizada para rastrear a última data de extração, garantindo que as transformações posteriores utilizem os dados mais recentes.

### 2. **logs**
A pasta `logs` é responsável por armazenar os logs de execução da auditoria. Cada execução dos processos de auditoria gera logs que são armazenados nesta pasta para monitoramento e análise de falhas ou erros durante o processo de transformação.

- **Função**: Armazena os logs de execução da auditoria.
- **Uso**: Permite a análise de falhas e performance, ajudando na resolução de problemas e no acompanhamento do processo de auditoria.

### 3. **pbm**
A pasta `pbm` é dedicada ao armazenamento dos arquivos do PBM extraídos do SharePoint. Esses arquivos são usados para transformações antes de serem carregados no banco de dados.

- **Função**: Armazena os arquivos do PBM extraídos do SharePoint.
- **Uso**: Serve para armazenar os arquivos que precisam passar por transformação antes de serem enviados ao banco de dados.

### 4. **planilhas**
A pasta `planilhas` guarda os arquivos relacionados a vendas extras, acessos e slow changing dimensions, também extraídos do SharePoint. Assim como a pasta `pbm`, esses arquivos são transformados antes de serem carregados no banco de dados.

- **Função**: Armazena os arquivos de vendas extras, acesso e slow changing dimensions extraídos do SharePoint.
- **Uso**: Processamento e transformação desses arquivos antes de enviá-los ao banco de dados.

### 5. **ticket medio**
A pasta `ticket medio` segue a mesma lógica das pastas `pbm` e `planilhas`, mas é focada em armazenar arquivos específicos relacionados ao ticket médio. Esses arquivos também são extraídos do SharePoint, transformados e posteriormente enviados ao banco de dados.

- **Função**: Armazena os arquivos relacionados ao ticket médio extraídos do SharePoint.
- **Uso**: Processamento e transformação dos arquivos relacionados a essa métrica antes do carregamento no banco de dados.

## Resumo da Estrutura

A estrutura da pasta `dados` está organizada para facilitar o armazenamento e a transformação de diferentes tipos de dados extraídos do SharePoint. Cada pasta tem uma função específica, seja para armazenar dados de auditoria, arquivos de PBM, ou arquivos relacionados a vendas e ticket médio. O processo de transformação e envio para o banco de dados ocorre após o armazenamento nesses diretórios.

