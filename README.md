analise-extratos 
# Análise de Extratos Bancários

## Conceitos

Requisitos que o projeto segue:
* Coleta da fonte de dados
* Realizar o ETL para destino no banco de dados
* Fazer a integração da Ferramenta de BI com o banco de dados
* Na ferramenta de BI, construir os gráficos visuais

## Visão Geral
<p>O Projeto foi desenvolvido visando demonstrar os principais gastos recorrente do cartão de crédito utilizado em dois modelos, Virtual e Físico. 
Ao longo do desenvolvimento do dashboard, foram criados métricas de gasto total e a sua média, como também o percentual de qual desses tipos de cartão foi mais utilizado. Também foi criado análise temporal, realizando o volume de gasto em qual dia do mês com mais movimentação, bem como as top categorias com maiores gastos médios.</p>

### Modelagem

<p>Para essa etapa utilizei a ferramenta <b>SQL Power Architect</b> para construção da modelagem que está sendo utilizada no negócio. Como é uma etapa inicial criei apenas uma tabela de dimensão, mas com o andar do tempo, pretendo incluir outras tabelas para relacionamento e então ter um conjunto de dados mais produtivo.</p>

### Fonte de Dados

<p>Para a realização desse projeto, foi necessário buscar as fontes a serem trabalhadas, e então vi que todas estavam no formato <i>".csv"</i> de todos os meses. 
Nesse caso, foi solicitado pelo aplicativo do banco a relação de gastos da fatura de cartão de crédito referente à todos os meses de 2023 até o momento
<li>Fatura Mês -- Formato CSV </li>
</p>

### ETL (Extração, Transformação e Carregamento) dos Dados

<p>Para essa etapa foi utilizada a ferramenta Pentaho Data Integration. Este me permitiu realizar toda a ação de construção de steps e toda a parte de configuração, desde a fonte de dados, transformações e então carregamento final no banco de dados MySQL.

Fiz a parte de buscar a fonte, em seguida usei o step para concatenar todos os dados em uma só. Depois desse passo usei o step para remoção de uma coluna que veio da step de concetanação. Por último da parte de transformação, utilizei o step de ordenação crescente de datas para serem salvas no banco de dados.

Após concluir toda as etapas, foi feita a carga dos dados no Banco.

![image](https://github.com/iuryml/analise-extratos/assets/55949523/c9216a51-fea7-4f92-9bac-9b04d22e2f16)

Imagem acima demonstra todos os steps de transformação até o carregamento final no banco de dados.
</p>

### Protipação do Dashboard

<p>Nessa etapa utilizei o <b>Figma</b> para realização da construção do design do Dashboard. Ela é muita rica em detalhes e demais edições que permite obter elaborar ótimas métricas e indicadores</p>

### Integração da ferramenta de BI com o Banco de Dados

<p>Nessa etapa foi utilizada a ferramenta <b>Microsoft Power BI</b> para início da construção do dashboard e demais medidas DAX necessárias. Mas antes dessa etapa, houve a integração com Banco de Dados MySQL e para isso, utilizei a fonte <b>ODBC</b> para conexão.
Criei a query no banco de dados para trazer o resultado da consulta dos campos que marquei como importantes para serem trabalhados e então, integrados os dados com o Power BI.</p>

Query utilizada
```sql
select nome_cartao, data_compra, numero_final_cartao, categoria, descricao, parcela, valor_BRL from extratos where categoria != '-'
-- Selecionando os principais campos para análise, retirando os dados que estão com a categoria vazia
```

### Desenvolvimento de Dashboard 

<p>Chegando a parte final do projeto onde foi realizada a construção do Dashboard para apresentação dos insights que consegui tirar.</p>

![image](https://github.com/iuryml/analise-extratos/assets/55949523/4a97e143-ab96-4296-bff0-720163178a7d)

#### Instruções para análise

<p>Vemos que foram criados 2 cards e nesses eu criei 1 medida DAX para realizar o cálculo total de gastos.
O gráfico de pizza foram utilizdas duas medidas, sendo 1 o percentual total de utilização do Cartão Físico e outra do Virtual.

Dois gráficos de colunas foram construídos, mas um demonstra um visão diferente do outro.<br><br>
O gráfico de <i>Dia dos Meses com Maior Movimentação Financeira</i> apresenta uma análise temporal de todas os dias dos meses que teve a maior movimentação de uso do Cartão de Crédito.<br>
Já o gráfico de <i>Gasto Total por Tipo de Cartão</i> apresenta resultados de qual tipo de cartão teve o maior volume durante os meses do ano.
</p>

<p>Por fim o gráfico de colunas onde apresentei as principais categorias e sua média de gastos. Coloquei uma médida personalizada para destacar os principais gastos com média acima da média de gastos.</p>

## Conclusão

Podemos ver que há uma grande movimentação do titular da conta durante os primeiros 15 dias em uso de Cartão Virtual, provavelmente para promoções de produtos ou demais outras utilidades que há na internet. Ao todo podemos destacar também que o Titular usa o cartão Físico para comprar de necessidade como para Supermercados, Roupas e Automotivos. Esse padrão foi encontrado durante a análise. 

Também podemos destacar que as principais categorias preferidas de gastos são Educacional, Automotivas e Vestuários.


