
|  | Hive | Spark | BigQuery |
|----------|----------|----------|----------|
| Função | Sistema de data warehouse distribuído e tolerante a falhas que permite análises em grande escala. | Estrutura de análise de dados que pode realizar análises complexas em memória em grandes volumes de dados de até petabytes. | Plataforma de análise de dados totalmente gerenciada e pronta para IA que ajuda a impulsionar o valor dos dados, funcionando com vários mecanismos, formatos e nuvens. |
| Tipo de processamento | Processamento em lote usando frameworks computacionais Apache Tez ou MapReduce. |  | Processamento interativo e distribuído usando o serviço Dremel para consultas SQL. |
| Latência | Média a alta, dependendo da capacidade de resposta do mecanismo de computação. O modelo de execução distribuída oferece performance superior em comparação com sistemas de consulta monolíticos, como o RDBMS, para os mesmos volumes de dados.  |
| Tipos de dados | Oferece suporte a dados estruturados e não estruturados. Fornece suporte nativo para tipos de dados SQL comuns, como INT, FLOAT e VARCHAR.  | 
| Tipo | Data Warehouse | Framework para Análise de dados | Data Warehouse |
| Formato armazenamento |  |  | Colunar |
| Linguagem de Consulta | HiveQL | SQL, Java, Scala, Python e R | SQL |
| Arquitetura | Hive Server 2 (aceita solicitações de usuários e aplicativos e cria planos de execução), Hive Query Language (HQL), Apache Hive Metastore externo (armazena todos os metadados do Hive) e Hive Beeline Shell. |  | Uma camada de armazenamento que ingere, armazena e otimiza dados e uma camada de computação que fornece recursos de análise |

https://aws.amazon.com/pt/what-is/apache-hive/

https://logz.io/blog/hive-vs-spark/

https://cloud.google.com/bigquery?hl=pt

https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/36632.pdf

https://www.databricks.com/br/glossary/apache-hive

https://spark.apache.org/

|  | HDFS | GFS (Colossus) |
|----------|----------|----------|
