# Passo a Passo para Criar Backup do PostgreSQL

## Passo 1: Instalar o pg_dump
O `pg_dump` vem com a instalação do PostgreSQL. Se você ainda não o instalou:

### Windows:
Baixe e instale o PostgreSQL no [site oficial](https://www.postgresql.org/download/).

### Linux:
Instale com:
```bash
sudo apt update && sudo apt install postgresql-client -y
```
---

## Passo 2: Criar Backup Manualmente

### Windows (Prompt de Comando ou PowerShell):
Abra o terminal e rode:
```bash
pg_dump -U usuario -h localhost -p 5432 -d meu_banco -F c -b -v -f "C:\Backups\backup.dump"
```
Caso o comando `pg_dump` não seja reconhecido, pode ser porque o PostgreSQL não está instalado ou o caminho do executável (bin do PostgreSQL) não está no Path do sistema. Para adicionar PostgreSQL ao Path do Sistema:

Abra o Painel de Controle e vá até:

- Sistema **>** Configurações Avançadas do Sistema **>** Variáveis de Ambiente

- Em Variáveis do Sistema, encontre Path e clique em Editar.

- Adicione o caminho da pasta bin do PostgreSQL. Exemplo:

```bash
C:\Program Files\PostgreSQL\<versao>\bin
```
Salve e reinicie o PowerShell. Agora, o comando pg_dump deve funcionar.

Após seguir os passos acima, rode:

```bash
pg_dump --version
```
Se aparecer a versão do pg_dump, significa que está funcionando. 

### Linux (ou WSL rodando no Windows):
Se estiver no WSL e quiser salvar no Windows:
```bash
pg_dump -U usuario -h localhost -p 5432 -d meu_banco -F c -b -v -f /mnt/c/Backups/backup.dump
```

🔹 Explicação:

  -  -U usuario → Nome do usuário do PostgreSQL.
  -  -h localhost → Host do banco.
  -  -p 5432 → Porta padrão.
  -  -d meu_banco → Nome do banco.
  - -F c → Formato compactado (.dump).
  -  -b → Inclui blobs.
  -  -v → Modo verboso.
  -  -f caminho_do_arquivo → Onde salvar o backup.

---

## Passo 3: Criar uma Rotina de Backup Automático

### Windows (Agendador de Tarefas):

- Abrir o Agendador de Tarefas (`taskschd.msc`).
- Criar uma Nova Tarefa.
- Em Ações, selecione Executar um Programa e insira:
    Programa: `C:\Program Files\PostgreSQL\<versao>\bin\pg_dump.exe`
    Argumentos:

```bash
-U usuario -h localhost -p 5432 -d meu_banco -F c -b -v -f C:\Backups\backup_%date:~-4,4%%date:~-7,2%%date:~-10,2%.dump
```

- Isso cria backups com a data no nome, ex: backup_20250211.dump.

---

### Linux (Crontab):
Abra o terminal e edite o crontab:

```bash
crontab -e
```
Adicione esta linha para rodar o backup de seg-sex às 10h da manhã:

```bash
0 10 * * 1-5 pg_dump -U usuario -h localhost -p 5432 -d meu_banco -F c -b -v -f /mnt/c/Users/User/OneDrive\ -\ EASE\ LABS\ GROUP/1\ -\ Operação\ -\ 327/15\ -\ Backups/backup_$(date +\%Y\%m\%d).dump
```
🔹 Salva o backup no Windows com a data no nome.

Abaixo como configurar outro horário de sua preferência no crontab

```lua
* * * * *
| | | | |
| | | | +-- Dia da semana (0 - 6) (Domingo = 0)
| | | +---- Mês (1 - 12)
| | +------ Dia do mês (1 - 31)
| +-------- Hora (0 - 23)
+---------- Minuto (0 - 59)
```

## Passo 4: Restaurar Backup

Para restaurar os dados em caso de perda, use:

Substitua o caminho com o caminho no qual você fez o backup

```bash
pg_restore -U usuario -h localhost -p 5432 -d meu_banco -v "/mnt/c/Backups/backup_20250211.dump"
```

Agora, o banco terá backups automáticos e estará seguro! 🚀