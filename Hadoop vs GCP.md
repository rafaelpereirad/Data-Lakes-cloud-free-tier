### OCI
<p align="center"> <img width="662" alt="image" src="https://github.com/user-attachments/assets/950c9b74-0991-4cd5-b9fb-ee2cd67f07b2">

### GCP  
<p align="center"> <img width="637" height="284" alt="image" src="https://github.com/user-attachments/assets/9d6b944e-9bce-4646-951e-0bdb51ae0d99" />

# Processamento

|  | Hive | Spark | BigQuery |
|----------|----------|----------|----------|
| Função | Sistema de data warehouse distribuído e tolerante a falhas que permite análises em grande escala. | Estrutura de análise de dados que pode realizar análises complexas em memória em grandes volumes de dados de até petabytes. | Plataforma de análise de dados totalmente gerenciada e pronta para IA que ajuda a impulsionar o valor dos dados, funcionando com vários mecanismos, formatos e nuvens. |
| Tipo de processamento | Processamento em lote usando frameworks computacionais Apache Tez ou MapReduce. | Processamento na memória, reduzindo o número de etapas em uma tarefa e reutilizando dados em várias operações paralelas. | Processamento interativo e distribuído usando o serviço Dremel para consultas. |
| Latência | Média a alta, dependendo da capacidade de resposta do mecanismo de computação. O modelo de execução distribuída oferece performance superior em comparação com sistemas de consulta monolíticos, como o RDBMS, para os mesmos volumes de dados.  | O Spark SQL é um mecanismo de consulta distribuído que fornece consultas interativas de baixa latência até 100 vezes mais rápidas que o MapReduce |  |
| Tipos de dados | Oferece suporte a dados estruturados e não estruturados. Fornece suporte nativo para tipos de dados SQL comuns, como INT, FLOAT e VARCHAR.  |  | Dados estruturados e semi-estruturados. As tabelas de objetos permitem analisar dados não estruturados no Cloud Storage. É possível fazer análises com funções remotas ou inferência usando o BigQuery ML e, em seguida, mesclar os resultados dessas operações com o restante dos seus dados estruturados no BigQuery.  * |
| Tipo | Data Warehouse | Framework para Análise de dados | Data Warehouse |
| Formato armazenamento | CSV, TSV, Sequencial, RCFile, Avro, ORC e Parquet | CSV, JSON, ORC, e Parquet | Colunar |
| Linguagem de Consulta | HiveQL | SQL, Java, Scala, Python e R | SQL |
| Arquitetura | Hive Server 2 (aceita solicitações de usuários e aplicativos e cria planos de execução), Hive Query Language (HQL), Apache Hive Metastore externo (armazena todos os metadados do Hive) e Hive Beeline Shell. | Driver Process (mantem as informações sobre o aplicativo Spark, responde à consulta do usuário e agenda e distribui tarefas aos executores), Cluster Manager e Executors (armazenamento dos dados e execução do código nos dados distribuídos) | Uma camada de armazenamento que ingere, armazena e otimiza dados e uma camada de computação que fornece recursos de análise |

*Guia de tradução de SQL do Apache Hive: 
O Apache Hive e o BigQuery têm sistemas de tipos de dados diferentes. Na maioria dos casos, é possível mapear tipos de dados no Hive para tipos de dados do BigQuery com algumas exceções, como MAP e UNION. O Hive é compatível com mais transmissões implícitas do que o BigQuery. Como resultado, o tradutor de SQL em lote insere muitas transmissões explícitas

https://cloud.google.com/bigquery/docs/migration/hive-sql?hl=pt-br

https://www.geoambiente.com.br/blog/bigquery-conheca-as-novas-capacidades-de-analise-de-dados-nao-estruturados-e-de-streaming-no-google-cloud/#:~:text=Como%20reflexo%2C%20o%20BigQuery%20foi,dados%20s%C3%A3o%20considerados%20n%C3%A3o%20estruturados.

Introdução a fontes de dados externas:
https://cloud.google.com/bigquery/docs/external-data-sources?hl=pt-br

 | YARN e Mapreduce                                                                                                                                                             | Dremel                                                                                                                                      |
 | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ |
| A função do YARN é dividir as funcionalidades de gerenciamento de recursos e de agendamento/monitoramento de tarefas em daemons separados.                                     | Linguagem de alto nível, semelhante a SQL, para expressar consultas ad hoc.                                                                 |
 | O MapReduce é um framework de software para escrever facilmente aplicações que processam grandes volumes de dados (conjuntos de dados de múltiplos terabytes) em paralelo em grandes clusters. | Executa consultas nativamente, sem traduzi-las para tarefas de MapReduce (MR jobs).                                                         |
 |                                                                                                                                                                              | Economia de armazenamento e redução do custo de CPU devido a uma compressão mais eficiente.                                                 |
 |                                                                                                                                                                              | Inspirou o Apache Drill (código aberto).                                                                                                    |
 |                                                                                                                                                                              | O Dremel oferece execução tolerante a falhas, um modelo de dados flexível e capacidades de processamento de dados *in situ* (no local).     |

https://siliconangle.com/2012/08/20/googles-dremel-here-comes-a-new-challenger-to-yarnhadoop/

https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html

# Armazenamento 

| Característica                        | HDFS (Hadoop Distributed File System)                                                                                                                                                                                                                                                                                                  | GFS (Google File System)                                                                                                                                                                                                                                                                                                                           |
| :------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Implementação** | Já o HDFS, baseado no projeto de código aberto Apache Hadoop, pode ser implantado e utilizado por qualquer empresa que deseje gerenciar e processar big data.                                                                                                                                                                            | Como o GFS é um sistema de arquivos proprietário e exclusivo do Google, ele не pode ser utilizado por nenhuma outra empresa.                                                                                                                                                                                                                       |
| **Serviço de Arquivos** | No Hadoop, o sistema de arquivos HDFS divide os arquivos em unidades chamadas blocos, com 128 MB de tamanho. O tamanho do bloco pode ser ajustado com base no tamanho dos dados.                                                                                                                                                             | No GFS, os arquivos são divididos em unidades de tamanho fixo chamadas *chunks*. O tamanho do *chunk* é de 64 MB e eles podem ser armazenados em diferentes nós no cluster para necessidades de balanceamento de carga e desempenho.                                                                                                                  |
| **Gerenciamento de Cache** | O HDFS possui o “DistributedCache”. O DistributedCache é um recurso fornecido pelo MapReduce para distribuir eficientemente arquivos grandes, somente leitura e específicos da aplicação. Ele também armazena em cache arquivos como texto, arquivos compactados (zip, tar, tgz e tar.gz) e jars necessários pelas aplicações. Os arquivos do DistributedCache podem ser privados ou públicos, o que determina como podem ser compartilhados nos nós escravos (*slave nodes*). | No GFS, os metadados de cache são salvos na memória do cliente. O servidor de *chunks* (*chunk server*) não precisa armazenar dados de arquivos em cache. O sistema Linux em execução no servidor de *chunks* armazena em cache na memória os dados acessados com frequência.                                                                               |
| **Proteção e Permissão de Arquivos** | O HDFS implementa um modo de permissão do tipo POSIX para arquivos e diretórios. Todos os arquivos e diretórios são associados a um proprietário e a um grupo, com permissões separadas para usuários que são proprietários, para usuários que são membros do grupo e para todos os outros usuários.                                                        | A Suitebriar - parceira do Google - menciona em sua pesquisa de análise de segurança que o GFS divide os arquivos e os armazena em várias partes em várias máquinas. Os nomes dos arquivos são aleatórios e não são legíveis por humanos. Os arquivos são ofuscados por meio de algoritmos que mudam constantemente.                                        |
| **Estratégia de Replicação** | O HDFS possui um sistema automático de replicação baseado em rack. Por padrão, duas cópias de cada bloco são armazenadas por DataNodes diferentes no mesmo rack, e uma terceira cópia é armazenada em um DataNode em um rack diferente.                                                                                                      | O GFS possui dois tipos de réplicas: réplicas primárias e réplicas secundárias. Uma réplica primária é o *chunk* de dados que um servidor de *chunks* envia a um cliente. As réplicas secundárias servem como backups em outros servidores de *chunks*. O usuário pode especificar o número de réplicas a serem mantidas.                                    |
| **Namespace de Arquivos** | O HDFS suporta uma organização de arquivos hierárquica tradicional. Usuários ou aplicações podem criar diretórios para armazenar arquivos. O HDFS também suporta sistemas de arquivos de terceiros, como CloudStore e Amazon Simple Storage Service (S3).                                                                                           | No GFS, os arquivos são organizados hierarquicamente em diretórios e identificados por nomes de caminho (*path names*). O GFS é exclusivo do Google.                                                                                                                                                                                                  |
| **Banco de Dados do Sistema de Arquivos** | Com base nos *white papers* de estudo do "Bigtable", a Apache desenvolveu seu próprio banco de dados chamado HBase no projeto de código aberto Hadoop. O HBase é construído com a linguagem Java. Existem grandes características em comum entre o Bigtable e o HBase.                                                                                   | O GFS possui o banco de dados Bigtable. O Bigtable é um banco de dados proprietário desenvolvido pelo Google usando C++.                                                                                                                                                                                                                                |


Escalabilidade: Tanto o HDFS quanto o GFS são considerados arquiteturas baseadas em cluster. Cada sistema de arquivos é executado sobre máquinas construídas com hardware comum (commodity hardware). Cada cluster pode consistir em milhares de nós com armazenamento para grandes volumes de dados.

Embora o GFS tenha sido inovador por si só, ele tinha limitações em termos de escalabilidade. Para resolver esses problemas, o Google desenvolveu o Colossus como uma versão aprimorada do GFS. O Colossus fornece armazenamento para vários produtos do Google e serve como a plataforma de armazenamento subjacente para os serviços do Google Cloud, tornando-o publicamente disponível. Com escalabilidade e disponibilidade aprimoradas, o Colossus é projetado para lidar com as demandas de dados em rápido crescimento das aplicações modernas.

Google File System:

<p align="center"> <img width="568" alt="image" src="https://github.com/user-attachments/assets/c416dce0-15cf-4887-8091-1543b75354f3">

https://www.infoq.com/articles/dfs-architecture-comparison/
Research Article: Analyzing Google File System and Hadoop Distributed File System, Nader Gemayel

# Ingestão

|  | Kafka | Pub/Sub |
|----------|----------|----------|
| Ordem das mensagens	| Em partições | Dentro de tópicos |
| Gerenciamento de infraestrutura	 | Implementar e operar manualmente máquinas virtuais (VMs). Mantenha versões e patches consistentes.	 | Gerenciado pelo Google. |
| Limite mensagens | Configurável | 10 MB |

<p align="center"> <img width="539" alt="image" src="https://github.com/user-attachments/assets/f76897b7-d07b-4603-8a46-194637d73e5a">

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
