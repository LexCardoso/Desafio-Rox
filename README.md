# Desafio-Rox
Desafio para vaga de Engenheiro de dados Júnior
Link Colab: https://colab.research.google.com/drive/1eR4OX82b6IEu_deO3k_P__C0ZDVC_c9N#scrollTo=8twmXZA6E6Xh

Colab nao esta sendo lido corretamente, compartilhei um arquivo copiado pelo vscode

Desafio:

O presente problema se refere aos dados de uma empresa que produz bicicletas. 
O objetivo deste desafio é compreender os seus conhecimentos e experiência analisando os seguintes aspectos:

- Fazer a modelagem conceitual dos dados;
- Criação da infraestrutura necessária;
- Criação de todos os artefatos necessários para carregar os arquivos para o banco criado;
- Desenvolvimento de SCRIPT para análise de dados;
- (opcional) Criar um relatório em qualquer ferramenta de visualização de dados.

Os seguintes arquivos devem ser importados (ETL) para o banco de dados de sua escolha: 

Sales.SpecialOfferProduct.csv
Production.Product.csv
Sales.SalesOrderHeader.csv
Sales.Customer.csv
Person.Person.csv
Sales.SalesOrderDetail.csv

## Análise de dados
Com base na solução implantada responda aos seguintes questionamentos:

1 - Escreva uma query que retorna a quantidade de linhas na tabela Sales.SalesOrderDetail pelo campo SalesOrderID, desde que tenham pelo menos três linhas de detalhes.

2 - Escreva uma query que ligue as tabelas Sales.SalesOrderDetail, Sales.SpecialOfferProduct e Production.Product e retorne os 3 produtos (Name) mais vendidos (pela soma de OrderQty), agrupados pelo número de dias para manufatura (DaysToManufacture).

3 - Escreva uma query ligando as tabelas Person.Person, Sales.Customer e Sales.SalesOrderHeader de forma a obter uma lista de nomes de clientes e uma contagem de pedidos efetuados.

4 - Escreva uma query usando as tabelas Sales.SalesOrderHeader, Sales.SalesOrderDetail e Production.Product, de forma a obter a soma total de produtos (OrderQty) por ProductID e OrderDate.

5 - Escreva uma query mostrando os campos SalesOrderID, OrderDate e TotalDue da tabela Sales.SalesOrderHeader. Obtenha apenas as linhas onde a ordem tenha sido feita durante o mês de setembro/2011 e o total devido esteja acima de 1.000. Ordene pelo total devido decrescente.


### Resolução

Todo o código do colab está comentado passo a passo. Abaixo vou descrever de forma geral todo o processo e quando necessário algumas imagens.


Decidi usar a GCP como serviço de nuvem pública, para a infraestrutura necessária para a realização do desafio eu criei: 

- conta de serviço através do IAM (chave JSON)
- instância do mysql
- database chamado bicicletas através do SHELL
- usuário do mysql através do console
- acesso ao ip da instância do colab
- bucket para armazenamento dos arquivos do desafio e arquivos salvos

Já no colab agrupei os dfs por módulo de tarefa, iniciei minhas análises usando o spark e anotando alguma inconsistências ou erros, após essa análise inicial criei o df para cada arquivo identificado como df_nome_arquivo pelo pandas, fiz alterações quando necessário, nem todos os dfs precisaram de tratamento, após isso conectei com a instância do MYsql na gcp ao colab e enviei os arquivos como tabela mas ainda sem relacionamento, uma vez que todos os df estão dentro do mysql fiz as ligações de Primary key e Foreign key.

Após validação de todas as PKs e FKs fiz as 5 querys do desafio no colab, depois de voltar algumas vezes nesse passo e tratar tudo que precisava, o próximo passo foi salvar esses arquivos no bucket na pasta TRUSTED 

Voltando para a gcp no big query, criei o database bicicletasbigquery, importei os arquivos do bucket TRUSTED e refiz a mesma query que fiz no colab agora na gcp para comparar os resultados.

Anotações e comentários.

- Precisei ajustar a coluna de ID do arquivo Sales.SpecialOfferProduct

- A ideia seria conectar direto do mysql e bigquery mas nao consegui finalizar esse passo então fiz o upload pelo bucket e não diretamente pela instância
