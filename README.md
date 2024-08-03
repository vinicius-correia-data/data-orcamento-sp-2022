##### AIRFLOW-DOCKER KARHUB #####
Este projeto tem como objetivo desenvolver um processo de ETL 
para processar os arquivos que representam o orçamento do Estado de São Paulo de 2022.

Para iniciar o docker :

INICIAR O DOCKER > docker-compose up -d
INICIAR O AIRFLOW WEBSERVER > docker-compose run webserver airflow db init

##### PASTAS #####
auth : Pasta para as credenciais  
dags : Pastas onde ficam os arquivos .py que contem a estrutura das dags
scripts : Pasta onde ficam os Scripts que serão acionados pelas dags 
database : Pasta onde ficam os arquivos CSV onde contem os dados que serão consumidos.

##### SCRIPTS ##### :

DATA_EXTRACT_DESPESAS.PY : Este script faz a leitura do CSV de DESPSESAS e CRIA uma tabela com os dados tratados e ajustados para criar uma tabela no bigquery caso ela não exista ou seja excluida.
DATA_EXTRACT_RECEITA.PY :  Este script faz a leitura do CSV de RECEITA e CRIA uma tabela com os dados tratados e ajustados para criar uma tabela no bigquery caso ela não exista ou seja excluida.
DOLAR.PY : Este script faz uma extração via api do valor do dolar.
CREATE_FINAL_TABLE.PY :  Este script faz a criação da tabela final. O codigo  python faz a consulta da query no bigquery e verifica se a tabela ainda existe, caso não , ele cria uma. Caso exista, ele conclui a tarefa.
RESPOSTAS_PERGUNTAS.PY : Esta dag, faz as consultas com as perguntas solicitadas, e responde de acordo com a pergunta. Nos logs, ele faz um print das respostas.

##### DAGS #####:

DAG_EXTRACT_DESPESAS.PY : Aciona, e executa o script DATA_EXTRACT_DESPESAS
DAG_EXTRACT_RECEITAS.PY : Aciona, e executa o script DATA_EXTRACT_RECEITA
DOLAR.PY : Aciona, e executa o script DOLAR
CREATE_TABLE_ORCAMENTO.PY : Aciona, e executa o script CREATE_FINAL_TABLE
DAG_RESPOSTAS_PERGUNTAS.PY : Aciona, e executa o script RESPOSTAS_PERGUNTAS

##### COMO CHEGUEI ATE AQUI #####

1 - Busquei guardar os dados em uma tabela para melhor entendimento de como eles estavam estruturados ( ARRECADAÇÃO, LIQUIDAÇÃO E DOLAR).
2 - Criei consultas separadas para ARRECADAÇÃO e LIQUIDAÇÃO para poder agrupar e ter os dados de acorodo com o solicitado.
3 - Criei uma consulta , fazendo um inner join entre as tabelas de ARRECADAÇÃO E LIQUIDAÇÃO por ID, e INNER join com o dolar por DATA para pegar a cotação do dia correto.
4 - Para respodner as dags da forma solicitada ( ultilizando consultas sql ), criei um script com as consutlas que geram as respostas , e coloquei em sequencia para printar nos LOGS do airflow as respostas. 

