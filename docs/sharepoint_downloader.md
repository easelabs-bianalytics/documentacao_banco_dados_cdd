# SharePoint Downloader

Este script é uma implementação de uma classe em Python para interagir com o SharePoint, permitindo a autenticação, obtenção de arquivos e download de arquivos de uma pasta remota no SharePoint para um diretório local. Ele usa a biblioteca `office365-rest-python-client` para interagir com o SharePoint e a biblioteca `dotenv` para carregar as credenciais de autenticação de um arquivo `.env`.

## Estrutura do Código

### Bibliotecas Importadas
- **os**: Usada para interagir com o sistema de arquivos.
- **office365.sharepoint.client_context**: Importa o contexto de cliente para interagir com o SharePoint.
- **office365.sharepoint.files.file**: Usada para trabalhar com arquivos no SharePoint.
- **office365.runtime.auth.authentication_context**: Usada para autenticar o usuário no SharePoint.
- **dotenv**: Biblioteca para carregar variáveis de ambiente a partir de um arquivo `.env`.

### Carregamento de Credenciais
O arquivo `.env` é carregado utilizando a função `load_dotenv()` para acessar as variáveis de ambiente que contêm o nome de usuário (`SHARE_USER`) e a senha (`SHARE_PASS`) para autenticação no SharePoint.

### URLs e Pastas Remotas
- `site_url`: Contém dois links para diferentes sites SharePoint.
- `pasta_remota`: Contém caminhos relativos para várias pastas no SharePoint onde os arquivos desejados podem ser encontrados.

## Classe `SharePointDownloader`

### Atributos
- **site_url**: URL do site SharePoint ao qual você deseja se conectar.
- **username** e **password**: Credenciais para autenticação no SharePoint.
- **ctx**: Um objeto `ClientContext` autenticado que é usado para realizar as operações de interação com o SharePoint.

### Métodos

- **`__init__(self, site_url, username, password)`**:
  Inicializa a classe, define os valores para `site_url`, `username`, e `password`, e chama o método `_autenticar()` para obter o contexto de conexão autenticado.

- **`_autenticar(self)`**:
  Realiza a autenticação no SharePoint utilizando o nome de usuário e senha fornecidos. Caso a autenticação seja bem-sucedida, retorna o contexto `ClientContext` para uso posterior.

- **`obter_arquivos_da_pasta(self, pasta_remota)`**:
  Obtém os arquivos de uma pasta específica no SharePoint. Retorna uma lista de arquivos encontrados na pasta.

- **`baixar_arquivo(self, arquivo_remoto, caminho_destino)`**:
  Baixa um arquivo individual do SharePoint para um caminho de destino local especificado.

- **`baixar_todos_arquivos(self, pasta_remota, pasta_local)`**:
  Baixa todos os arquivos de uma pasta remota do SharePoint para um diretório local.

## Como Funciona o Código

1. **Autenticação**: Quando a classe `SharePointDownloader` é instanciada, ela tenta autenticar o usuário no SharePoint usando as credenciais fornecidas. Se a autenticação for bem-sucedida, o contexto de cliente é criado.

2. **Obter Arquivos**: Para obter arquivos de uma pasta, o método `obter_arquivos_da_pasta()` é chamado com o caminho relativo da pasta no SharePoint. O SharePoint retorna uma lista de arquivos na pasta, que é então utilizada para fazer o download dos arquivos.

3. **Baixar Arquivos**: O método `baixar_arquivo()` baixa um arquivo específico do SharePoint para o diretório local. O caminho do arquivo remoto no SharePoint e o caminho local onde o arquivo será salvo são fornecidos como parâmetros.

4. **Baixar Todos os Arquivos**: O método `baixar_todos_arquivos()` é utilizado para baixar todos os arquivos de uma pasta. Ele chama `obter_arquivos_da_pasta()` para listar os arquivos e, em seguida, usa `baixar_arquivo()` para baixar cada arquivo individualmente.


## Conclusão

Essa implementação pode ser utilizada em vários cenários para automatizar o processo de obtenção e download de arquivos de um SharePoint. Ele oferece uma maneira simples de conectar-se ao SharePoint, autenticar-se e fazer o download de arquivos para o sistema local.

