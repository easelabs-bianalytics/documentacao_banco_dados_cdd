# Guia de Atualização da Documentação com MkDocs e GitHub Pages

Este guia descreve o processo para atualizar a documentação do seu projeto utilizando MkDocs, armazenando os arquivos Markdown na branch `main` e publicando a documentação estática na branch `gh-pages`.

---

## 1. Instalar o MkDocs

Caso ainda não tenha o MkDocs instalado, execute:

```bash
pip install mkdocs
```

Se desejar utilizar o tema Material para MkDocs, instale também:

```bash
pip install mkdocs-material
```

## 2. Comitar as Mudanças na Branch `main`

Certifique-se de que todos os arquivos .md e a configuração do MkDocs estão atualizados na branch main. Caso necessário, execute os comandos:

```bash
git add .
git commit -m "Atualizando a documentação"
git push origin main
```

## 3. Gerar os Arquivos Estáticos

Gere os arquivos estáticos da documentação com:

```bash
mkdocs build
```

Isso criará os arquivos HTML e outros recursos na pasta `site`.

## 4. Publicar a Documentação na Branch gh-pages
Para enviar a documentação gerada para a branch gh-pages, utilize:

```bash
poetry run mkdocs gh-deploy
```

**O que esse comando faz**:
  - Cria a branch gh-pages (caso não exista).
  - Copia os arquivos estáticos da documentação para essa branch.
  - Faz um commit e envia os arquivos para o repositório remoto na branch gh-pages.

## 5. Verificar a Documentação no GitHub Pages
Após o deploy, a documentação estará disponível na URL:

```bash
https://easelabs-bianalytics.github.io/documentacao_banco_dados_cdd/
```
Aguarde alguns minutos para que o GitHub Pages processe as alterações.

## 6. Resolvendo Problemas Comuns

**Alterações não aparecem no GitHub Pages?**
- Verifique se a branch gh-pages está sendo atualizada corretamente.
- Confirme que a opção GitHub Pages está habilitada no repositório (Settings > Pages).
- Force uma atualização com:

```bash
mkdocs gh-deploy --clean
```
