# Migração e Transferência de Dados na AWS

A AWS oferece um portfólio abrangente de serviços para ajudar a migrar dados, bancos de dados, aplicações e até mesmo servidores inteiros do seu ambiente on-premises para a nuvem, ou entre diferentes regiões da AWS. A escolha do serviço correto depende do volume de dados, da velocidade da sua conexão de rede e da natureza da migração (online vs. offline).

## Categorias de Serviços de Migração

Os serviços de migração e transferência podem ser divididos em duas categorias principais:

-   **Dispositivos Físicos (Transferência Offline):** Para grandes volumes de dados onde a transferência pela internet não é viável. A AWS envia um dispositivo físico para sua localidade.
    -   **AWS Snow Family (Snowcone, Snowball Edge, Snowmobile)**

-   **Serviços Gerenciados (Transferência Online):** Para transferências de dados pela rede, migrações de banco de dados e integração híbrida.
    -   **AWS Storage Gateway**
    -   **AWS DataSync**
    -   **AWS Database Migration Service (DMS)**
    -   **Amazon Kinesis**

A seguir, detalhamos cada um desses serviços.

## 1. AWS Storage Gateway

O AWS Storage Gateway é um serviço híbrido que conecta seus ambientes de aplicações on-premises ao armazenamento na nuvem da AWS. Ele fornece uma ponte segura e de baixa latência entre seus sistemas locais e os serviços de armazenamento da AWS, como S3, EBS e Glacier.

Existem três tipos principais de gateways:

-   **Gateway de Arquivos (File Gateway):**
    -   **Como funciona:** Apresenta um endpoint de compartilhamento de arquivos (usando protocolos NFS ou SMB) para seus servidores on-premises. Os arquivos gravados nesse compartilhamento são armazenados como objetos no Amazon S3.
    -   **Caso de uso:** Migrar dados de servidores de arquivos para o S3, fazer backup de arquivos na nuvem e fornecer acesso de baixa latência a dados na nuvem para aplicações on-premises (usando um cache local).

-   **Gateway de Fitas (Tape Gateway):**
    -   **Como funciona:** Emula uma biblioteca de fitas de backup virtual (VTL) em seu ambiente on-premises. Ele se integra com seu software de backup existente. As fitas virtuais são armazenadas no S3 e podem ser arquivadas no S3 Glacier ou S3 Glacier Deep Archive.
    -   **Caso de uso:** Substituir fitas de backup físicas por uma solução na nuvem, eliminando o custo e a complexidade do gerenciamento de fitas físicas.

-   **Gateway de Volumes (Volume Gateway):**
    -   **Como funciona:** Apresenta volumes de armazenamento em bloco (usando o protocolo iSCSI) para seus servidores on-premises.
    -   **Modos:**
        -   **Volumes armazenados (Stored Volumes):** Armazena o conjunto de dados completo localmente e faz backup assíncrono de snapshots desses dados no S3 (na forma de snapshots do EBS). Ideal para acesso de baixa latência a todo o conjunto de dados.
        -   **Volumes em cache (Cached Volumes):** Armazena o conjunto de dados completo no S3 e mantém apenas os dados acessados com mais frequência em cache localmente. Ideal para economizar espaço de armazenamento local.

## 2. AWS DataSync

O AWS DataSync é um serviço de transferência de dados online que simplifica, automatiza e acelera a movimentação de grandes volumes de dados entre o armazenamento on-premises (NFS, SMB, HDFS), outros provedores de nuvem e os serviços de armazenamento da AWS (S3, EFS, FSx).

-   **Principais Características:**
    -   **Aceleração:** Usa um protocolo de rede otimizado e compressão para transferir dados até 10 vezes mais rápido que ferramentas de código aberto.
    -   **Segurança:** Criptografa os dados em trânsito.
    -   **Automação:** Gerencia automaticamente a transferência, incluindo validação de integridade dos dados, agendamento de tarefas e monitoramento.
    -   **Como funciona:** Você implanta um "agente" do DataSync como uma máquina virtual em seu ambiente on-premises. Este agente se conecta aos seus sistemas de armazenamento e transfere os dados de forma segura e eficiente para a AWS.

    ## 3. AWS Snow Family

A Família AWS Snow é projetada para a transferência de dados em escala de petabytes (ou até exabytes) usando dispositivos físicos e seguros. É a solução ideal quando a transferência de dados pela internet não é viável devido ao tempo, custo ou limitações de largura de banda.

-   **AWS Snowcone:**
    -   Dispositivo ultracompacto, portátil e robusto.
    -   **Capacidade:** 8 TB (HDD) ou 14 TB (SSD) de armazenamento.
    -   **Recursos:** Possui poder de computação (CPU e memória) para executar cargas de trabalho de edge computing (usando AWS IoT Greengrass ou instâncias EC2).

-   **AWS Snowball Edge:**
    -   Dispositivo maior e mais robusto, disponível em duas opções:
        -   **Storage Optimized:** Otimizado para armazenamento, com até 80 TB de capacidade (HDD). Ideal para grandes transferências de dados.
        -   **Compute Optimized:** Otimizado para computação, com mais poder de CPU, memória e GPU opcional. Ideal para processamento de dados localmente em ambientes desconectados ou remotos.

-   **AWS Snowmobile:**
    -   Um caminhão contêiner de 45 pés para transferências em escala de exabytes.
    -   **Capacidade:** Até 100 PB por Snowmobile.
    -   **Caso de uso:** Migração de data centers inteiros, lagos de dados massivos ou arquivos de vídeo.

## 4. AWS Database Migration Service (DMS)

O AWS DMS é um serviço gerenciado que ajuda a migrar bancos de dados para a AWS de forma rápida e segura. O banco de dados de origem permanece totalmente operacional durante a migração, minimizando o tempo de inatividade para as aplicações que dependem dele.

-   **Principais Características:**
    -   **Migrações Homogêneas e Heterogêneas:**
        -   **Homogênea:** Migração entre bancos de dados do mesmo tipo (ex: MySQL on-premises para Amazon RDS for MySQL).
        -   **Heterogênea:** Migração entre diferentes tipos de banco de dados (ex: Oracle on-premises para Amazon Aurora).
    -   **Schema Conversion Tool (SCT):** Para migrações heterogêneas, a SCT é usada em conjunto com o DMS. Ela converte o esquema do banco de dados de origem (tabelas, visões, stored procedures) para um formato compatível com o banco de dados de destino.
    -   **Replicação Contínua de Dados (Change Data Capture - CDC):** Após a carga inicial dos dados, o DMS pode replicar continuamente as alterações do banco de dados de origem para o de destino, permitindo uma transição (cutover) com tempo de inatividade mínimo.

<p align="center">
    <img alt="Fluxo de replicação AWS DMS" width="640" src="https://docs.aws.amazon.com/pt_br/dms/latest/userguide/images/datarep-Welcome.png" />
    <br/><em>Fonte: AWS DMS User Guide</em>
</p>


## 5. Amazon Kinesis

O Amazon Kinesis é uma família de serviços projetada para coletar, processar e analisar dados de streaming em tempo real. É ideal para lidar com grandes volumes de dados gerados continuamente, como logs de aplicações, dados de clickstream de websites, feeds de redes sociais e dados de dispositivos IoT.

-   **Kinesis Data Streams:**
    -   **Função:** É o serviço principal para captura e armazenamento de streams de dados. Os dados são armazenados em "shards" por um período de retenção (geralmente 24 horas, mas pode ser estendido para até 365 dias).
    -   **Como funciona:** "Produtores" (como servidores web, aplicações móveis) enviam dados para o stream. "Consumidores" (como instâncias EC2, funções Lambda) leem e processam esses dados a partir dos shards em tempo real.
    -   **Caso de uso:** Análise de logs em tempo real, processamento de dados de jogos, ingestão de dados para machine learning.

-   **Kinesis Data Firehose:**
    -   **Função:** É a maneira mais fácil de carregar dados de streaming em data stores e ferramentas de análise. Ele captura, transforma e carrega os dados de forma totalmente gerenciada.
    -   **Como funciona:** Você aponta o Firehose para um stream de dados (pode ser um Kinesis Data Stream ou dados enviados diretamente para o Firehose) e o configura para entregar esses dados a um destino como Amazon S3, Amazon Redshift, Amazon OpenSearch Service ou Splunk. Ele pode agrupar, comprimir e transformar os dados (ex: de JSON para Parquet) antes da entrega.
    -   **Diferença para o Data Streams:** O Firehose é focado na *entrega* (load) para um destino, enquanto o Data Streams é focado no *processamento* em tempo real por consumidores customizados.

-   **Kinesis Data Analytics:**
    -   **Função:** Permite processar e analisar dados de streaming em tempo real usando SQL padrão ou Apache Flink.
    -   **Como funciona:** Você pode executar consultas SQL contínuas sobre os dados em um Kinesis Data Stream ou Kinesis Data Firehose para filtrar, transformar e agregar os dados em tempo real.
    -   **Caso de uso:** Criar painéis de análise em tempo real, gerar alertas baseados em métricas de streaming e realizar análises de séries temporais.

## 6. AWS Glue

O AWS Glue é um serviço de extração, transformação e carga (ETL) totalmente gerenciado que facilita a preparação e o carregamento de dados para análise. Ele é "serverless", o que significa que não há infraestrutura para gerenciar.

-   **Principais Componentes:**
    -   **Data Catalog:** É um repositório de metadados centralizado. O Glue pode usar "crawlers" para descobrir automaticamente seus dados em serviços como Amazon S3, Amazon RDS e DynamoDB, inferir seus esquemas e popular o Data Catalog. Este catálogo pode então ser usado por outros serviços como Amazon Athena, Amazon Redshift Spectrum e Amazon EMR.
    -   **Jobs ETL:** O Glue gera automaticamente código Python ou Scala (usando Apache Spark) para seus trabalhos de ETL, que você pode customizar. Esses trabalhos podem ser agendados, acionados por eventos ou executados sob demanda para transformar e mover dados.
    -   **Glue DataBrew:** É uma ferramenta de preparação de dados visual que permite limpar e normalizar dados sem escrever código. É ideal para analistas de dados e cientistas de dados.

-   **Casos de Uso:**
    -   **ETL para Data Lakes:** O caso de uso mais comum é extrair dados de várias fontes, transformá-los em um formato otimizado (como Apache Parquet) e carregá-los em um data lake no Amazon S3.
    -   **Descoberta de Dados:** Usar crawlers para manter um catálogo atualizado de todos os seus ativos de dados.
    -   **Limpeza e Preparação de Dados:** Usar o DataBrew para preparar dados para análise e machine learning.

## 7. Amazon Athena

O Amazon Athena é um serviço de consulta interativo e serverless que permite analisar dados armazenados no Amazon S3 usando SQL padrão. Com o Athena, não há necessidade de carregar seus dados em um banco de dados; você pode consultá-los diretamente onde estão.

-   **Principais Características:**
    -   **Serverless:** Não há servidores para provisionar ou gerenciar. O Athena cuida de tudo automaticamente.
    -   **Pagamento por Consulta:** Você paga apenas pelas consultas que executa, com base na quantidade de dados escaneados pela consulta.
    -   **Integração com AWS Glue:** O Athena usa o AWS Glue Data Catalog para armazenar metadados (como esquemas de tabelas) sobre os dados no S3. Quando um crawler do Glue cataloga seus dados, você pode consultá-los imediatamente no Athena.
    -   **Formatos de Dados:** Suporta uma variedade de formatos de dados, incluindo CSV, JSON, ORC, Avro e Parquet. O uso de formatos colunares (como Parquet e ORC) e a compressão de dados podem otimizar significativamente a performance e reduzir os custos das consultas.

-   **Casos de Uso:**
    -   **Análise de Logs:** Executar consultas ad-hoc em logs de aplicações, logs de acesso a load balancers ou logs do CloudTrail armazenados no S3.
    -   **Querying de Data Lakes:** Servir como a principal ferramenta de SQL para analistas de negócios e cientistas de dados que precisam explorar o data lake da empresa.
    -   **Fonte de Dados para BI:** Conectar ferramentas de Business Intelligence, como o Amazon QuickSight, ao Athena para criar dashboards e visualizações a partir de dados no S3.
```