
# Processamento

|  | Hive | Spark | BigQuery |
|----------|----------|----------|----------|
| Função | Sistema de data warehouse distribuído e tolerante a falhas que permite análises em grande escala. | Estrutura de análise de dados que pode realizar análises complexas em memória em grandes volumes de dados de até petabytes. | Plataforma de análise de dados totalmente gerenciada e pronta para IA que ajuda a impulsionar o valor dos dados, funcionando com vários mecanismos, formatos e nuvens. |
| Tipo de processamento | Processamento em lote usando frameworks computacionais Apache Tez ou MapReduce. | Processamento na memória, reduzindo o número de etapas em uma tarefa e reutilizando dados em várias operações paralelas. | Processamento interativo e distribuído usando o serviço Dremel para consultas SQL. |
| Latência | Média a alta, dependendo da capacidade de resposta do mecanismo de computação. O modelo de execução distribuída oferece performance superior em comparação com sistemas de consulta monolíticos, como o RDBMS, para os mesmos volumes de dados.  | O Spark SQL é um mecanismo de consulta distribuído que fornece consultas interativas de baixa latência até 100 vezes mais rápidas que o MapReduce |  |
| Tipos de dados | Oferece suporte a dados estruturados e não estruturados. Fornece suporte nativo para tipos de dados SQL comuns, como INT, FLOAT e VARCHAR.  | 
| Tipo | Data Warehouse | Framework para Análise de dados | Data Warehouse |
| Formato armazenamento | CSV, TSV, Sequencial, RCFile, Avro, ORC e Parquet | CSV, JSON, ORC, e Parquet | Colunar |
| Linguagem de Consulta | HiveQL | SQL, Java, Scala, Python e R | SQL |
| Arquitetura | Hive Server 2 (aceita solicitações de usuários e aplicativos e cria planos de execução), Hive Query Language (HQL), Apache Hive Metastore externo (armazena todos os metadados do Hive) e Hive Beeline Shell. | Driver Process (mantem as informações sobre o aplicativo Spark, responde à consulta do usuário e agenda e distribui tarefas aos executores), Cluster Manager e Executors (armazenamento dos dados e execução do código nos dados distribuídos) | Uma camada de armazenamento que ingere, armazena e otimiza dados e uma camada de computação que fornece recursos de análise |



# Armazenamento 

|  | HDFS | GFS (Colossus) |
|----------|----------|----------|


# Ingestão

|  | Kafka | Pub/Sub |
|----------|----------|----------|
| Ordem das mensagens	| Em partições | Dentro de tópicos |
| Gerenciamento de infraestrutura	 | Implementar e operar manualmente máquinas virtuais (VMs) ou máquinas. Mantenha versões e patches consistentes.	 | Gerenciado pelo Google |
| Limite mensagens | Configurável | 10 MB |

<img width="539" alt="image" src="https://github.com/user-attachments/assets/f76897b7-d07b-4603-8a46-194637d73e5a">

+ No diagrama anterior, cada M representa uma mensagem.
+ Os agentes do Kafka gerenciam várias partições ordenadas de mensagens, representadas pelas linhas horizontais de mensagens.
+ Os consumidores leem mensagens de uma partição específica que tem capacidade com base na máquina que hospeda essa partição.
+ O Pub/Sub não tem partições, e os consumidores leem de um tópico que realiza o escalonamento automático de acordo com a demanda.
+ Você configura cada tópico do Kafka com o número de partições necessárias para processar a carga de consumidor esperada.
+ O Pub/Sub é escalonado automaticamente com base na demanda.

https://aws.amazon.com/pt/what-is/apache-hive/

https://logz.io/blog/hive-vs-spark/

https://cloud.google.com/bigquery?hl=pt

https://cwiki.apache.org/confluence/display/Hive/FileFormats

https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/36632.pdf

https://www.databricks.com/br/glossary/apache-hive

https://spark.apache.org/

https://blog.dsacademy.com.br/serie-spark-e-databricks-parte-1-arquitetura-e-componentes-do-apache-spark/

https://cloud.google.com/pubsub/docs/migrating-from-kafka-to-pubsub?hl=pt-br
