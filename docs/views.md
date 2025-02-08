# Explicação das Views Criadas

## 1. View: `extra_consolidado`
### Descrição
Esta view consolida os dados de vendas extras, agrupando as quantidades por data, código de contrato (CT) e código de apresentação.
### Lógica de Negócio
- Soma as quantidades (`QTD`) das vendas extras por data, CT e apresentação.
- Atribui um valor padrão `99999` para `cod_ct` caso não esteja presente.
- Realiza um LEFT JOIN com as tabelas SCD de território para CT e GR, utilizando `cod_ct` e `cod_gr`, além do período correspondente. Dessa forma, soma apenas as quantidades vendidas enquanto os respectivos CTs e GRs eram responsáveis pelo território. Isso garante a unicidade dos dados por período, já que um mesmo território pode ter sido gerenciado por diferentes CTs e GRs em momentos distintos.
---

## 2. View: `acesso_consolidado`
### Descrição
Esta view agrupa os acessos por data, código de CT e produto, separando as quantidades por modalidade (`Mercado Publico` e `Saúde Suplementar`).
### Lógica de Negócio
- Calcula a quantidade de frascos por modalidade.
- Realiza um LEFT JOIN com as tabelas SCD de território para CT e GR, utilizando `cod_ct` e `cod_gr`, além do período correspondente. Dessa forma, soma apenas as quantidades vendidas enquanto os respectivos CTs e GRs eram responsáveis pelo território. Isso garante a unicidade dos dados por período, já que um mesmo território pode ter sido gerenciado por diferentes CTs e GRs em momentos distintos.
---

## 3. View: `vendas_consolidado`
### Descrição
Consolida as vendas do CDD por mês, informante, apresentação e ponto de venda.
### Lógica de Negócio
- Exclui vendas hospitalares (uma vez que o intuito dessa view é computar vendas que ocorreram devido à influência de CTs) e dos informantes `DIMED DISTR`, `LS`, `PORTAL`, `FARMACIAS ASSOCIADAS` (por serem redundantes).
- Converte unidades e valores para a escala correta (o CDD computa uma unidade com 1000).
- Realiza um LEFT JOIN com as tabelas SCD de território para CT e GR, utilizando `cod_ct` e `cod_gr`, além do período correspondente. Dessa forma, soma apenas as quantidades vendidas enquanto os respectivos CTs e GRs eram responsáveis pelo território. Isso garante a unicidade dos dados por período, já que um mesmo território pode ter sido gerenciado por diferentes CTs e GRs em momentos distintos.
---

## 4. View: `pbm_consolidado`  
### Descrição  
Consolida os dados de vendas do PBM (Programa de Benefício em Medicamentos), considerando os descontos aplicados e ajustes na quantidade vendida.  

### Lógica de Negócio  
- Filtra descontos superiores a **70%**.  
- A partir de **janeiro/2025**, os descontos acima de **70%** passaram a ser deduzidos das vendas do CT. No entanto, até **dezembro/2024**, apenas descontos **≥ 99%** eram considerados. O filtro foi ajustado para refletir essa regra.  
- A cláusula SQL:  

    ```sql
    CASE WHEN fp."DESC ADM" BETWEEN 70 AND 80 THEN "QTDE" * 0.67 ELSE "QTDE" END AS qtde
    ```

    Implementa a lógica de aplicação do desconto de **70%**, na qual, a cada **três vouchers** de **70%**, **duas vendas** são descontadas. Isso resulta em um fator de ajuste de **2/3**, substituindo a quantidade original por `"QTDE" * 0.67`, que representa a quantidade efetivamente reduzida no total de vendas do CT.  

- Realiza um **LEFT JOIN** com as tabelas **SCD de território** para **CT** e **GR**, utilizando `cod_ct`, `cod_gr` e o período correspondente. Dessa forma, as quantidades são somadas apenas durante os períodos em que os respectivos **CTs** e **GRs** eram responsáveis pelo território. Isso garante a unicidade dos dados por período, considerando que um mesmo território pode ter sido gerenciado por diferentes **CTs** e **GRs** em momentos distintos.  


---

## 5. View: `vw_sell_out`
### Descrição  
Consolida todas as fontes de dados de vendas — `vendas_consolidado`, `extra_consolidado`, `acesso_consolidado` e `pbm_consolidado` — para oferecer uma visão unificada do **sell-out**. O resultado é uma tabela que apresenta o número de vendas, organizadas em colunas separadas para **CDD, vendas extras, mercado público e saúde suplementar**, segmentadas por **dia, CT e GR**.  
Essa tabela captura a **dinamicidade das relações entre território, CT e GR**, sendo a principal fonte de dados para o **dashboard de Metas e Resultados**.  

### Lógica de Negócio  
- Agrega os valores provenientes das diferentes origens de vendas.  
- Garante a **coerência na granularidade temporal e geográfica**.  
- Relaciona-se com o **ticket médio** para o cálculo dos valores das vendas.  


---

## Considerações Finais
- Todas as views asseguram a integridade dos dados através de joins com as tabelas de território e gerenciamento.
- Foram aplicadas regras de tratamento para casos nulos e valores padrão (`99999` para `cod_ct` e `9999` para `cod_gr`).
- A estrutura visa otimizar consultas analíticas e fornecer uma visão confiável das vendas e acessos.

