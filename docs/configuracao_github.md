# Manual de Instruções: Clonando Repositório e Configuração de Autenticação SSH

## Objetivo

Este manual irá orientá-lo na configuração da autenticação SSH para acessar o GitHub e na clonagem do repositório remoto para começar a trabalhar no projeto localmente.

### Passo 1: **Verificar se o Git está instalado**

Primeiro, verifique se o Git está instalado na sua máquina. Abra o terminal e execute o comando:

```bash
git --version
```

Se o Git não estiver instalado, siga as instruções para instalá-lo no [site oficial do Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

### Passo 2: **Configuração de chave SSH**

Para configurar a autenticação SSH e garantir que você possa acessar o repositório remoto no GitHub sem precisar inserir sua senha a cada vez, siga os seguintes passos:

#### 2.1: **Gerar uma nova chave SSH**

Execute o comando abaixo para gerar uma nova chave SSH. Substitua seu e-mail associado ao GitHub.

```bash
ssh-keygen -t rsa -b 4096 -C "seu-email@dominio.com"
```

Pressione **Enter** para aceitar o local padrão para salvar a chave.

#### 2.2: **Adicionar a chave SSH ao agente SSH**

Execute o seguinte comando para iniciar o agente SSH, caso ainda não esteja em execução:

```bash
eval "$(ssh-agent -s)"
```

Adicione sua chave SSH privada ao agente com o comando abaixo:

```bash
ssh-add ~/.ssh/id_rsa
```

#### 2.3: **Adicionar chave SSH à sua conta do GitHub**

Obtenha a chave SSH pública com o comando:

```bash
cat ~/.ssh/id_rsa.pub
```

Copie o conteúdo gerado.

Agora, acesse o GitHub:

1. Vá para **Settings** (Configurações).
2. Clique em **SSH and GPG keys** no menu à esquerda.
3. Clique em **New SSH key**.
4. Cole sua chave SSH pública copiada na caixa **Key** e dê um nome identificável.
5. Clique em **Add SSH key**.

### Passo 3: **Testar a Conexão SSH com o GitHub**

Execute o comando abaixo para testar a conexão com o GitHub:

```bash
ssh -T git@github.com
```

Se a conexão for bem-sucedida, você verá uma mensagem como:

```bash
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Passo 4: **Clonar o Repositório Remoto**

Agora, com a autenticação SSH configurada, você pode clonar o repositório do GitHub usando o método SSH.

No GitHub, acesse a página do repositório que deseja clonar, clique no botão **Code** e copie a URL de clonagem SSH.

No terminal, execute o seguinte comando para clonar o repositório:

```bash
git clone git@github.com:easelabs-bianalytics/atualizacao_banco_dados_cdd.git
```

### Passo 5: **Configuração do Repositório Local**

Após clonar o repositório, navegue até o diretório do repositório clonado:

```bash
cd atualizacao_banco_dados_cdd
```

Agora, você pode começar a trabalhar no seu repositório local.

### Passo 6: **Sincronizando Alterações**

#### 6.1: **Fazendo alterações locais**

Sempre que você fizer alterações no seu repositório local, execute os seguintes comandos para salvar e enviar as alterações para o repositório remoto.

1. **Adicionar alterações ao índice**:
    ```
    git add .
    ```
    Isso adiciona todas as alterações ao índice para preparação.

2. **Fazer commit das alterações**:
    ```
    git commit -m "Descrição do que foi alterado"
    ```

3. **Enviar as alterações para o repositório remoto**:
    ```
    git push origin main
    ```

#### 6.2: **Baixando alterações do repositório remoto**

Se houver alterações no repositório remoto que você ainda não tem no seu repositório local, execute o comando abaixo para baixar essas alterações:

```bash
git pull origin main
```

Isso irá garantir que seu repositório local esteja sempre atualizado com as alterações do repositório remoto.

---

Com esses passos, você estará pronto para interagir com seu repositório remoto usando SSH e manter a sincronização entre as versões local e remota.





