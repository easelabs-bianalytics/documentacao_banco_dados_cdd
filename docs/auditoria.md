# Auditoria de Discrepâncias e Sincronização de Dados

## Introdução
Este documento descreve o processo de auditoria de discrepâncias entre os bancos de dados de origem e destino. O objetivo é identificar inconsistências nos dados e, se necessário, sincronizar as informações.

## Configuração Inicial
O código carrega variáveis de ambiente e configura os motores de banco de dados:

- **`engine_origem`**: Conexão com o banco de origem.

- **`engine_destino`**: Conexão com o banco de destino.

- **Variáveis de ambiente**:
    - `SENDER_EMAIL`
    - `PASSWORD_EMAIL`

O logging é configurado para registrar os eventos da auditoria.

## Estrutura de Dados
A tabela principal utilizada é `fato_cdd`, localizada no esquema `cddd`. Os principais campos considerados são:

- **`cod_anomes`**: Data no formato `YYYYMMDD`.
- **`und`**: Unidade a ser somada.

## Funções Implementadas

### `calcular_somas_mensais`
Calcula as somas mensais da tabela no banco de origem ou destino.

**Parâmetros:**

- `engine`: Conexão com o banco de dados.

- `tabela`: Nome da tabela.

- `data_col`: Coluna de data.

- `unidade_col`: Coluna de unidades.

- `origem_destino`: Define se os dados são da origem ou destino.

### `deletar_mes`
Deleta os registros do mês no qual a discrepância foi identificada na tabela de destino.

**Parâmetros:**

- `mes`: Mês no formato `YYYY-MM`.

- `engine_destino`: Conexão com o banco de destino.

- `tabela`: Nome da tabela.

- `data_col`: Coluna de data.

### `enviar_email`
Envia notificações por e-mail sobre discrepâncias e sincronizações.

**Parâmetros:**

- `subject`: Assunto do e-mail.

- `body`: Corpo do e-mail.

- `to_email`: Destinatário.

### `auditoria`
Executa a auditoria entre os bancos de dados de origem e destino.

**Passos do processo:**

1. Obtém os totais mensais de unidades para origem e destino.

2. Compara os valores e identifica discrepâncias.

3. Caso haja discrepância:
    - Registra no log.
    - Sincroniza os dados se a data mínima do mês na origem for o primeiro dia.
    - Envia notificação por e-mail.

## Conclusão
Esse processo permite garantir a integridade dos dados entre os sistemas. Se discrepâncias forem encontradas, ações corretivas são tomadas automaticamente.
