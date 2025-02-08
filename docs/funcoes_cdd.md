# Funções 

As funções presentes neste script são responsáveis por executar operações de integração e processamento de dados, abrangendo tarefas como manipulação de arquivos e interação com bancos de dados. Elas são amplamente utilizadas em diversas partes do projeto e foram desenvolvidas com o objetivo de eliminar redundâncias no código, promovendo maior reutilização e organização.

## Estrutura do Script

### 1. **Bibliotecas Importadas**

O script faz uso de várias bibliotecas para gerenciar as operações de banco de dados e manipulação de dados:

- `os`: Para operações de sistema de arquivos.
- `hashlib`: Para gerar hashes SHA-256.
- `datetime`: Para trabalhar com datas e horários.
- `pandas`: Para manipulação de dados e leitura de arquivos.
- `sqlalchemy`: Para interagir com bancos de dados SQL.
- `snowflake.sqlalchemy`: Para conectar ao Snowflake.
- `dotenv`: Para carregar variáveis de ambiente a partir de um arquivo `.env`.

### 2. **Funções Definidas**

#### `configurar_engine(schema)`
Configura a conexão com o banco de dados Snowflake, utilizando credenciais e informações armazenadas em variáveis de ambiente. A função retorna um objeto `engine` para interação com o banco de dados.

#### `carregar_dados(query, engine)`
Executa uma consulta SQL e retorna os resultados em um `DataFrame` do pandas.

#### `gerar_hash(row, colunas=None)`
Gera um hash SHA-256 com base nas colunas especificadas de uma linha de dados ou de toda a linha se nenhuma coluna for especificada.

#### `inserir_dados_com_tratamento(df, nome_tabela, engine_destino)`
Insere dados de um `DataFrame` em uma tabela de banco de dados PostgreSQL com tratamento de duplicados. A função utiliza a cláusula `ON CONFLICT` para evitar duplicação de dados com base no campo `hash`.

#### `drop_views(engine_destino)`
Exclui views específicas no banco de dados de destino (PostgreSQL), removendo dependências associadas com a cláusula `CASCADE`.

#### `recreate_views(engine_destino)`
Recria as views específicas no banco de dados de destino (PostgreSQL) executando as queries necessárias para cada view.

#### `execucao_no_periodo_permitido()`
Verifica se o dia atual está dentro de uma janela de execução permitida, definida entre os dias 15 e o último dia do mês.

#### Funções de Manipulação de Data:
- **`carregar_ultima_data(data_file)`**: Carrega a última data salva de um arquivo.
- **`salvar_ultima_data(data_file, data)`**: Salva a data atual em um arquivo.

#### `pasta_local(subpasta)`
Retorna o caminho do diretório local onde os arquivos devem ser salvos, criando a pasta, se necessário.

### 3. **Funções de Processamento de Arquivos**

#### `processar_arquivo(caminho_destino, aba)`
Lê uma aba específica de um arquivo Excel e reescreve a aba no mesmo arquivo, mantendo o formato original. Utiliza a biblioteca `pandas` e `openpyxl` para manipulação de arquivos Excel.

#### `determinar_modal(entidade)`
Classifica uma entidade em categorias como "B2B", "M. PÚBLICO", "VAREJO" ou "Indefinido", dependendo do nome da entidade.

#### `determinar_produto(nome)`
Classifica o tipo de produto com base no nome fornecido. As categorias podem incluir "ISOLADO 10 ML", "ISOLADO 30 ML", "EXTRATO 36,76", ou "Indefinido".

### 4. **Variáveis de Configuração**

- **`DATABASE_URL_DESTINO`**: A URL de conexão com o banco de dados PostgreSQL de destino.
- **`engine_destino`**: O objeto `engine` configurado para conexão com o PostgreSQL.

### 5. **Carregamento de Variáveis de Ambiente**

O script utiliza o arquivo `.env` para carregar variáveis sensíveis como credenciais de banco de dados e outras configurações importantes:

```bash
ACCOUNT=example_account
USER=example_user
PASSWORD_SNOW=example_password
DATABASE=example_database
SCHEMA=example_schema
WAREHOUSE=example_warehouse
ROLE=example_role
```