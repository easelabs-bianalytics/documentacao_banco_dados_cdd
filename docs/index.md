# Guia do Usuário

## 1. Introdução
  - **Visão Geral do Projeto**: 
      A criação deste projeto surgiu da necessidade de integrar e preservar dados históricos, uma vez que o CDD-Mensal disponibiliza apenas os últimos dois anos de informações até o mês anterior, sem atualizações diárias, enquanto o CDD-Diário possui as atualizações diárias mas mantém dados apenas dos últimos três meses. Dessa forma, não conseguíamos acompanhar nossa performance tanto diária quanto histórica a partir de uma única fonte de dados.

      Para solucionar esse problema, inicialmente extraímos uma carga completa do CDD-Mensal e, a partir disso, realizamos atualizações incrementais diárias com base no CDD-Diário. Esse processo é aplicado tanto às tabelas fato quanto às dimensões.

      Além das vendas registradas no CDD, também carregamos no banco de dados informações adicionais, como extras, acesso e PBM. Outra fonte incorporada ao banco, mas que não é atualizada diariamente, é o TD, que recebe atualizações apenas do 15º dia até o último dia do mês (período dentro do qual a Close-up atualiza a base).

      Em resumo, todas essas fontes de dados passam por um processo de Extração (coleta dos dados da origem), Transformação (ajustes necessários para garantir a padronização e integridade dos dados) e Carregamento (inserção no banco de destino), garantindo um fluxo contínuo e estruturado para a análise de desempenho.

- **Principais Funcionalidades**: A estrutura do projeto conta com as seguintes funcionalidades: 
      - Carregamento inicial de dados: Primeira carga realizada e já executada
      - Atualizações diárias
      - Auditoria: Conferência e validação dos dados em relação ao banco de origem

- **Público-Alvo**: Esse guia se destina a equipe de Business Analytics e todos aqueles envolvidos no desenvolvimento da infra-estrutura (administradores de banco de dados, engenheiros de dados, desenvolvedores, etc.) de dados da Ease Labs.

## 2. Pré-requisitos
* **VSCode**: Você pode usar ou IDE se preferir

* **Git e GitHub**:
    - Você deve ter o Git instalado em sua máquina. [Instruções de instalação do Git aqui](https://git-scm.com/book/pt-br/v2).
    - Você também deve ter uma conta no GitHub. [Instruções de criação de conta no GitHub aqui](https://docs.github.com/pt/get-started/onboarding/getting-started-with-your-github-account).
    - Se você for usuário Windows, recomendo esse vídeo: [Youtube](https://www.youtube.com/watch?v=_hZf1teRFNg).
    - Tutorial de Git e Github básico [Ebook](https://www.linkedin.com/feed/update/urn:li:activity:7093915148351864832/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7093915148351864832%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29&originTrackingId=4GUdvXH4TK%2BtZtlNHmiqJA%3D%3D).
    - Se você já é usuário Git, recomendo o vídeo do Akita: [Youtube](https://www.youtube.com/watch?v=6Czd1Yetaac).

* **Pyenv**: É usado para gerenciar versões do Python. [Instruções de instalação do Pyenv aqui](https://github.com/pyenv/pyenv#installation). Vamos usar nesse projeto o Python 3.12.0. Para usuários Windows, é recomendado assistirem esse tutorial [Youtube](https://www.youtube.com/watch?v=TkcqjLu1dgA).

* **Poetry**: Este projeto utiliza Poetry para gerenciamento de dependências. [Instruções de instalação do Poetry aqui](https://python-poetry.org/docs/#installation).Se você é usuário Windows, recomendo assistir esse vídeo: [Youtube](https://www.youtube.com/watch?v=BuepZYn1xT8). Que instala o Python, Poetry e VSCode. Mas um simples comando PIP INSTALL POETRY já resolve.


* **Permissões de Acesso**: 
    - Banco de dados Ease Labs no Snowflake 
    - Sharepoint EaseLabs 
    - Banco de dados de destino
    - Conta do gmail easelabs.analytics 


### Instalação e Configuração

Só prossiga para o passo-a-passo abaixo caso já tenha configurado a autenticação SSH para acessar o GitHub, caso contrário [consulte a documentação de configuração aqui](configuracao_github.md)

####1. Clone o repositório:

```bash
git clone git@github.com:easelabs-bianalytics/atualizacao_banco_dados_cdd.git
cd atualizacao_banco_dados_cdd
```

#### 2. Configure a versão correta do Python com `pyenv`:

```bash
pyenv install 3.12.0
pyenv local 3.12.0
```

####3. Configurar poetry para Python version 3.12.0 e ative o ambiente virtual:

```bash
poetry env use 3.12.0
poetry shell
```

####4. Instale as dependencias do projeto:

```bash
poetry install
```

####5. Crie um arquivo .env na raiz do projeto e copie e cole as credenciais de login

```bash
SHARE_USER=usuario@easelabs.com.br
SHARE_PASS=SuaSenha

SENDER_EMAIL=seuemail@gmail.com
PASSWORD_EMAIL=SuaSenha

ACCOUNT=SuaAccount
USER=SeuUser
PASSWORD_SNOW=SuaSenha
DATABASE=SeuDatabase
WAREHOUSE=SuaWarehouse
ROLE=SeuRole

RAILWAY_DB_USER=dbuser
RAILWAY_DB_PASS=dbsenha
RAILWAY_DB_HOST=dbhost
RAILWAY_DB_PORT=dbport
RAILWAY_DB_NAME=dbname
```

####7. Execute o comando de execucão da pipeline para realizar a ETL:

```bash
task run
```

####8. Verifique no banco se as tabelas foram atualizadas corretamente.


## 4. Processo de Carregamento de Dados
- **Carregamento Inicial de Dados**:
  - Descreva os passos para extrair os dados iniciais (primeiro carregamento) da fonte de dados.
  - Forneça comandos ou scripts de exemplo usados para o carregamento inicial.
  - Explique as transformações ou limpezas de dados rmkdocealizadas durante esse processo.
  - Inclua erros comuns e soluções para o carregamento inicial.
  
- **Atualizações Incrementais Diárias**:
  - Descreva o processo para as atualizações diárias, incluindo como os novos dados são extraídos, transformados e carregados no banco de dados.
  - Explique como o sistema detecta novos ou dados alterados (por exemplo, usando carimbos de data/hora, chaves incrementais).
  - Inclua a programação para as atualizações e como modificá-la.
  - Destaque como tratar erros ou problemas nas atualizações incrementais (por exemplo, tentativas, logs).

## 5. Execução do Código
- **Fluxo de Execução**: Uma explicação detalhada de como rodar os scripts ou ferramentas que realizam a extração, transformação e carregamento dos dados.
- **Agendamento**: Caso as atualizações sejam agendadas, explique como elas são automatizadas (por exemplo, usando cron jobs, Airflow ou um agendador de tarefas).
- **Logs e Monitoramento**: Forneça orientações sobre como o usuário pode monitorar o sucesso ou falha das atualizações, incluindo localizações de arquivos de log e códigos de erro.

## 6. Tratamento de Erros e Solução de Problemas
- **Problemas Comuns**: Liste erros típicos ou problemas que podem ocorrer durante a extração, transformação ou carregamento de dados.
- **Soluções e Contornos**: Forneça possíveis soluções para os problemas listados.
- **Dicas de Depuração**: Oriente os usuários sobre como depurar o processo, incluindo como verificar logs e reexecutar etapas específicas.

## 7. Manutenção e Otimizações
- **Manutenção do Banco de Dados**: Explique como realizar a manutenção regular do banco de dados, como otimizar índices, realizar vacuum ou purgar dados antigos.
- **Otimizações de Desempenho**: Ofereça dicas para melhorar o desempenho tanto do carregamento inicial quanto das atualizações incrementais (por exemplo, processamento paralelo, estratégias de indexação ou particionamento).
- **Escalabilidade**: Se aplicável, explique como escalar o sistema para conjuntos de dados maiores ou fontes de dados adicionais.

## 8. Considerações de Segurança
- **Segurança dos Dados**: Discuta as etapas para garantir a segurança dos dados sensíveis (por exemplo, criptografia, chamadas de API seguras).
- **Controle de Acesso**: Explique como gerenciar o acesso ao banco de dados e garantir que apenas usuários autorizados possam executar o processo de carregamento de dados.
- **Chaves de API e Credenciais**: Explique como armazenar e gerenciar com segurança chaves de API, senhas ou credenciais necessárias para acessar as fontes de dados.

## 9. Melhores Práticas
- **Qualidade de Código**: Forneça recomendações sobre como manter o código limpo, legível e fácil de manter.
- **Estratégia de Backup**: Recomende como fazer backup do banco de dados e dos dados antes e após o carregamento inicial e durante as atualizações incrementais.
- **Testes**: Encoraje a realização de testes tanto na lógica de carregamento inicial quanto nas atualizações incrementais. Sugira testes unitários, de integração e verificações de validação de dados.


