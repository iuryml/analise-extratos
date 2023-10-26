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

#### Métricas alocadas aos Cards
![image](https://github.com/iuryml/analise-extratos/assets/55949523/e1fb9c05-a043-421a-910b-d37e0aef23b7)

<p>Vemos que foram criados 3 cards principais no desenvolvimento e nesles criei 1 medida DAX para realizar o cálculo de <i>Gasto Total</i> e outra medida para calcular o <i>Gasto Médio por Dia</i></p>
<p>A métrica de <i>Taxa de Uso por Cartão</i> foi criada para mostrar o percentual de maior volume de preferência por tipo de Cartão.</p>

#### Principais Indicadores de Movimentação por Dias da Semana
![image](https://github.com/iuryml/analise-extratos/assets/55949523/fe3bc2b9-f6ff-4b29-84a3-1bcc6f250489)

<p>O gráfico de área <i>Maior Movimentação Financeira por Dias da Semana</i> apresenta uma análise temporal dos dias da semana teve a maior movimentação de uso do Cartão de Crédito.<br><br>
Aloquei uma linha média para definir um ponto de análise para entender quais dias tiveram um volume acima ou abaixo dessa média e de fato, as Sextas-Feiras superando os demais dias, com maior volume.</p>

#### Principais Indicadores de Gastos Mensais
![image](https://github.com/iuryml/analise-extratos/assets/55949523/afa1db74-723f-4188-bd53-efd963650b71)

<p>O gráfico de colunas demonstra o volume de gastos por ordem dos Meses do Ano e como destaque o valor<br><br>
Outro destaque foi o tooltip criado para demonstrar as principais Categorias com maior gasto referente aquele Mês</p>

#### Principais Categorias com Maior Gasto
![image](https://github.com/iuryml/analise-extratos/assets/55949523/43148085-9c36-4580-88d6-f189421e1ba3)

<p>O gráfico de Barras demonstrando por ordem acrescente de Gastos, as top principais Categorias que até o momento estão dominando o topo de Gastos</p>

## Conclusão

Podemos ver que há uma grande movimentação do titular da conta nos dias de Sextas Feiras, ambos tipos de uso de Cartões são utilizadas, dominando o topo de gastos, provavelmente para promoções de produtos ou demais outras utilidades que há neste dia. Ao todo podemos destacar também que o Titular usa o cartão Físico para comprar o que é de necessidade, como fato, os Supermercados, Roupas e Automotivos. Esse padrão foi encontrado durante a análise. 

Também podemos destacar que as principais categorias preferidas de gastos são **Educacional**, **Automotivas** e **Vestuários**.
