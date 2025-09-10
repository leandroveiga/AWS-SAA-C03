# Bancos de Dados na AWS (Atualizado SAA-C03 2025)

## Multi-AZ e Read Replica

A AWS (Amazon Web Services) oferece uma variedade de serviços de banco de dados para atender às necessidades dos usuários. Dois desses serviços são o Multi-AZ e o Read Replica, que fornecem recursos adicionais para melhorar a disponibilidade e a escalabilidade dos bancos de dados.

### Multi-AZ

O Multi-AZ é uma solução de alta disponibilidade para bancos de dados na AWS. Com o Multi-AZ, é possível criar uma réplica síncrona do banco de dados em uma zona de disponibilidade secundária. Essa réplica é mantida atualizada em tempo real com o banco de dados principal.

Se ocorrer uma falha no banco de dados principal, o Multi-AZ automaticamente promove a réplica para se tornar o novo banco de dados principal. Isso garante que o banco de dados esteja sempre disponível, minimizando o tempo de inatividade.

O Multi-AZ é amplamente utilizado para bancos de dados relacionais, como o Amazon RDS (Relational Database Service), que suporta MySQL, PostgreSQL, Oracle, SQL Server e outras opções de banco de dados.

**Exemplos**

- Suponha que você esteja executando um aplicativo crítico que depende de um banco de dados MySQL. Ao configurar o banco de dados MySQL no Amazon RDS com a opção Multi-AZ, você terá uma réplica síncrona do banco de dados em uma zona de disponibilidade secundária.

- Se ocorrer uma falha no banco de dados principal devido a um problema de hardware, por exemplo, o Multi-AZ automaticamente promoverá a réplica para se tornar o novo banco de dados principal. Isso garante que seu aplicativo continue funcionando sem interrupções, pois a réplica já está atualizada e pronta para ser usada.

### Read Replica

O Read Replica é uma solução de escalabilidade para bancos de dados na AWS. Com o Read Replica, é possível criar cópias assíncronas do banco de dados principal em diferentes regiões ou zonas de disponibilidade.

Essas cópias podem ser usadas para distribuir a carga de leitura do banco de dados, permitindo que consultas de leitura sejam executadas em réplicas em vez do banco de dados principal. Isso melhora o desempenho e a capacidade de resposta do aplicativo, pois a carga é distribuída entre várias instâncias de banco de dados.

O Read Replica é amplamente utilizado para bancos de dados relacionais, como o Amazon RDS, que suporta MySQL, PostgreSQL, Oracle, SQL Server e outras opções de banco de dados.

**Exemplos**

- Suponha que você esteja executando um aplicativo com alto tráfego de leitura, como um site de notícias. Ao configurar o banco de dados MySQL no Amazon RDS com a opção Read Replica, você pode criar várias réplicas assíncronas do banco de dados principal em diferentes regiões.

- As réplicas podem ser usadas para distribuir a carga de leitura, permitindo que consultas de leitura sejam executadas em réplicas em vez do banco de dados principal. Isso melhora o desempenho do aplicativo, pois a carga é distribuída entre várias instâncias de banco de dados.


## Backup, Multi-AZ e Read Replica na AWS

Backup, Multi-AZ e Read Replica. Esses recursos são amplamente utilizados para garantir a disponibilidade, a durabilidade e a escalabilidade de bancos de dados e sistemas na nuvem.

### Backup

O Backup é um recurso essencial para garantir a segurança dos dados armazenados na AWS. Com o Backup, é possível criar cópias dos dados e armazená-las em um local seguro, para que possam ser restauradas em caso de perda ou corrupção dos dados originais.

A AWS oferece diferentes opções de backup, como o Amazon RDS (Relational Database Service) para bancos de dados, o Amazon S3 (Simple Storage Service) para armazenamento de objetos e o Amazon EBS (Elastic Block Store) para armazenamento de blocos.

**Exemplo**:
- Para realizar o backup de um banco de dados MySQL hospedado no Amazon RDS, você pode usar o recurso de snapshots do RDS. Basta criar um snapshot do banco de dados e armazená-lo em um local seguro, como o Amazon S3.

### Multi-AZ

O Multi-AZ é um recurso que permite a replicação síncrona de um banco de dados em diferentes zonas de disponibilidade dentro de uma região da AWS. Isso garante alta disponibilidade e durabilidade dos dados, pois, em caso de falha em uma zona, o banco de dados é automaticamente failover para a zona de backup.

O Multi-AZ está disponível para bancos de dados hospedados no Amazon RDS, como MySQL, PostgreSQL, Oracle, SQL Server, entre outros.

**Exemplo**:
- Se você tem um banco de dados MySQL hospedado no Amazon RDS com o Multi-AZ habilitado, o RDS irá automaticamente replicar os dados para uma zona de backup. Em caso de falha na zona principal, o RDS irá promover a zona de backup para principal, garantindo a continuidade do serviço.

### Read Replica

O Read Replica é um recurso que permite a criação de cópias de leitura de um banco de dados, para distribuir a carga de leitura entre várias instâncias. Isso melhora o desempenho do banco de dados, pois as consultas de leitura podem ser distribuídas entre as réplicas.

O Read Replica está disponível para bancos de dados hospedados no Amazon RDS, como MySQL, PostgreSQL, Oracle, SQL Server, entre outros.

**Exemplo**:
- Se você tem um banco de dados MySQL hospedado no Amazon RDS e precisa lidar com um grande volume de consultas de leitura, você pode criar uma ou mais réplicas de leitura. As réplicas de leitura irão receber as consultas de leitura, aliviando a carga da instância principal.

## Introdução ao Amazon DynamoDB

O Amazon DynamoDB é um serviço de banco de dados NoSQL totalmente gerenciado pela AWS. Ele oferece escalabilidade, desempenho e alta disponibilidade para aplicativos que precisam de armazenamento de dados flexível e rápido.

**Exemplo:** 
- Se você estiver construindo um aplicativo da web que lida com dados variáveis e precisa de latência muito baixa, o DynamoDB é uma opção excelente.

### Estrutura de Dados Flexível

DynamoDB é um banco de dados NoSQL, o que significa que não possui um esquema rígido e permite que você armazene dados sem uma estrutura predefinida. Isso é útil para aplicativos que têm requisitos de armazenamento de dados em constante evolução.

**Exemplo:** 
- Se você estiver construindo um sistema de gerenciamento de conteúdo, pode armazenar diferentes tipos de objetos, como postagens de blog, comentários e imagens, sem a necessidade de esquemas complexos.

### Desempenho Escalável

DynamoDB oferece desempenho escalável. Você pode ajustar dinamicamente a capacidade de leitura e gravação para atender às necessidades de tráfego do seu aplicativo, garantindo que ele funcione sem problemas, mesmo com picos de uso.

**Exemplo:** 
- Se você estiver administrando um aplicativo de comércio eletrônico e esperar um aumento nas vendas durante a temporada de férias, poderá aumentar a capacidade do DynamoDB temporariamente para acomodar o aumento do tráfego.

### Gerenciamento Automático

O DynamoDB é totalmente gerenciado pela AWS, o que significa que a AWS cuida da administração do banco de dados, como ajuste de desempenho, backups e manutenção. Isso permite que você se concentre em desenvolver seu aplicativo em vez de lidar com tarefas de administração.

**Exemplo:** 
- Você não precisa se preocupar com a configuração de servidores, a otimização de índices ou a realização de backups regulares - o DynamoDB cuida disso para você.

### Modelo de Dados Flexível

O DynamoDB suporta modelos de dados flexíveis, incluindo documentos e pares chave-valor. Isso oferece a flexibilidade de escolher o modelo de dados que melhor atende às necessidades do seu aplicativo.

**Exemplo:** 
- Se você estiver desenvolvendo um aplicativo de análise de big data, pode usar o modelo de pares chave-valor para armazenar dados de forma eficiente.

### Replicação Global

O DynamoDB permite a replicação global, o que significa que você pode distribuir seus dados em várias regiões da AWS para alta disponibilidade e baixa latência em todo o mundo.

**Exemplo:** 
- Se seu aplicativo tiver usuários em diferentes partes do mundo, a replicação global pode garantir que todos tenham acesso rápido aos dados, independentemente de sua localização.

### Integração com Outros Serviços AWS

O DynamoDB se integra perfeitamente com outros serviços AWS, como AWS Lambda, AWS Step Functions, e AWS AppSync, permitindo a criação de aplicativos altamente escaláveis e com recursos poderosos.

**Exemplo:** 
- Você pode usar o AWS Lambda para acionar eventos em tempo real com base nas atualizações no DynamoDB, criando aplicativos reativos e orientados por eventos.

### Custo Baseado no Uso

O DynamoDB oferece um modelo de preços flexível com base no uso real. Você paga apenas pelo armazenamento e capacidade de leitura/gravação provisionada que utiliza, o que torna o serviço econômico para aplicativos de qualquer tamanho.

**Exemplo:** 
- Se você tiver um aplicativo inicialmente pequeno, não precisará pagar por recursos que não está usando. À medida que seu aplicativo cresce, você pode ajustar os recursos de acordo com suas necessidades.

## Leituras Consistentes e Leituras Fortemente Consistentes

No Amazon DynamoDB, há dois tipos de leitura disponíveis: leituras consistentes e leituras fortemente consistentes. Ambos têm um impacto significativo na forma como os dados são recuperados de uma tabela.

### Leituras Consistentes

As leituras consistentes no DynamoDB são a opção padrão e oferecem uma alta disponibilidade e baixa latência. Elas garantem que você obtém uma versão do dado que está consistente com as operações de gravação que ocorreram no passado em um curto período de tempo.

**Exemplo:** 
- Suponha que um item na tabela tenha sido atualizado recentemente. Se você executar uma leitura consistente, receberá a versão mais recente, mas não necessariamente a versão mais atualizada após cada gravação.

As leituras consistentes são adequadas para muitos casos de uso e podem proporcionar um excelente desempenho.

### Leituras Fortemente Consistentes

As leituras fortemente consistentes garantem que você obtenha a versão mais atualizada do dado, refletindo todas as operações de gravação, mesmo as mais recentes. Isso é alcançado às custas de uma latência ligeiramente maior e uma possível redução no desempenho em comparação com as leituras consistentes.

**Exemplo:** 
- Se um item na tabela foi atualizado muito recentemente e você executa uma leitura fortemente consistente, você obterá a versão mais atualizada após a gravação, garantindo a integridade dos dados.

As leituras fortemente consistentes são adequadas quando a integridade dos dados é crucial, e você não pode comprometer a obtenção da versão mais atualizada dos dados.

No DynamoDB, você pode escolher o tipo de leitura ao consultar a tabela. Você pode optar por leituras consistentes ou fortemente consistentes com base nos requisitos específicos do seu aplicativo.

É importante notar que o uso de leituras fortemente consistentes pode afetar o desempenho e a latência da consulta, portanto, é essencial equilibrar os requisitos de consistência com as necessidades de desempenho do seu aplicativo.

## Amazon ElastiCache: Melhorando o Desempenho com Cache na Nuvem

O Amazon ElastiCache é um serviço gerenciado de cache na nuvem que permite a implantação e a escalabilidade de infraestruturas de cache de forma eficiente. Ele suporta mecanismos de cache amplamente utilizados, como o Redis e o Memcached, proporcionando desempenho rápido e confiável para aplicativos.

### Benefícios do Amazon ElastiCache

O uso do Amazon ElastiCache oferece diversos benefícios:

- **Melhor Desempenho**: O cache permite armazenar dados frequentemente acessados em memória, proporcionando tempos de resposta mais rápidos para aplicativos.

- **Escalabilidade**: É possível dimensionar horizontalmente a infraestrutura de cache para atender às demandas crescentes de tráfego.

- **Redução da Carga no Banco de Dados**: Ao armazenar dados em cache, você reduz a carga nos bancos de dados, otimizando o acesso a informações frequentemente utilizadas.

- **Alta Disponibilidade**: O ElastiCache oferece opções de replicação e failover para garantir alta disponibilidade e tolerância a falhas.

- **Fácil Gerenciamento**: O serviço é totalmente gerenciado pela AWS, o que simplifica tarefas de manutenção e operação.

### Mecanismos de Cache Suportados

O Amazon ElastiCache oferece suporte para dois mecanismos de cache amplamente utilizados:

#### Redis

O Redis é um mecanismo de cache em memória altamente popular. Ele é conhecido por sua velocidade e flexibilidade, além de oferecer suporte a recursos avançados, como estruturas de dados complexas.

#### Memcached

O Memcached é outra opção poderosa para cache na memória. É uma escolha sólida para aplicativos que necessitam de alta taxa de transferência e compartilhamento de dados em cache de maneira simples.

### Cenários de Uso

O Amazon ElastiCache é adequado para uma variedade de cenários de uso, incluindo:

- **Melhoria de Desempenho**: Reduza a latência e melhore o desempenho de aplicativos ao armazenar em cache dados frequentemente acessados.

- **Alívio de Carga em Bancos de Dados**: Reduza a carga no banco de dados principal, permitindo que ele se concentre em operações críticas.

- **Escalabilidade**: Dimensione horizontalmente o cache para acomodar picos de tráfego.

- **Aplicativos em Tempo Real**: Suporte a aplicativos que exigem baixa latência e alto desempenho em tempo real.

## Amazon Redshift

O **Amazon Redshift** é um serviço de armazenamento de dados gerenciado que oferece alta performance para análise de dados. Ele é projetado para consultas de grandes conjuntos de dados e fornece uma solução escalável e econômica para data warehousing.

### Principais Características do Amazon Redshift

- **Armazenamento em Colunas**: O Amazon Redshift armazena dados em colunas, o que permite consultas mais rápidas e eficientes em grandes conjuntos de dados.

- **Escalabilidade**: É possível aumentar ou diminuir a capacidade de armazenamento e computação do Redshift de acordo com as necessidades do seu negócio.

- **Performance**: O Redshift é otimizado para consultas de alto desempenho e pode lidar com grandes volumes de dados de maneira eficaz.

- **Integração com Ferramentas de BI**: Ele é compatível com várias ferramentas de Business Intelligence (BI) e pode ser facilmente integrado ao seu ambiente de análise de dados.

- **Segurança**: Oferece recursos avançados de segurança, como criptografia de dados em repouso e em trânsito.

- **Single Node e Cluster**: O Redshift oferece opções de implantação, incluindo Single Node para cargas de trabalho menores e Cluster para cargas de trabalho de maior escala.

- **Compressão de Dados**: Utiliza compressão avançada para economizar espaço de armazenamento e acelerar as consultas.

- **Arquitetura Massivamente Paralela (MPP)**: O Redshift utiliza uma arquitetura MPP que divide as tarefas de consulta em várias unidades de processamento, acelerando o desempenho das consultas.
 
> Observação: Clusters RA3 e Redshift Serverless suportam recursos de resiliência avançada, incluindo cross-AZ dentro da região (Multi-AZ para melhor disponibilidade) e snapshots automáticos multi-região (quando configurados). A antiga limitação de "não é Multi-AZ" aplica-se a modelos legados.

### Data Warehousing

O Amazon Redshift é uma escolha popular para data warehousing devido à sua capacidade de lidar com análises complexas em grandes conjuntos de dados. Ele permite que as empresas armazenem, consultem e analisem dados de maneira eficaz, auxiliando na tomada de decisões informadas.

### Arquitetura Massivamente Paralela

O Redshift utiliza uma arquitetura massivamente paralela (MPP) que divide as tarefas de consulta em várias unidades de processamento, acelerando o desempenho das consultas. Isso é fundamental para análises rápidas em grandes volumes de dados.

### Armazenamento Colunar

Os dados no Redshift são armazenados em colunas em vez de linhas, o que permite consultas mais eficientes. Ele usa compressão avançada para economizar espaço de armazenamento e acelerar as consultas.

### Escalabilidade

Uma das vantagens do Redshift é a escalabilidade. Você pode aumentar ou diminuir a capacidade de armazenamento e computação conforme necessário, o que é particularmente útil para lidar com picos de carga.

### Integração com Ferramentas de BI

O Redshift é compatível com uma ampla variedade de ferramentas de Business Intelligence (BI), como Tableau, Power BI e outras. Isso facilita a criação de painéis de controle e relatórios interativos.

### Segurança

A segurança é uma prioridade no Redshift. Ele oferece criptografia de dados em repouso e em trânsito, autenticação baseada em credenciais e integração com o AWS Identity and Access Management (IAM) para controle de acesso refinado.

## Amazon Aurora

O **Amazon Aurora** é um serviço de banco de dados relacional compatível com MySQL e PostgreSQL desenvolvido pela Amazon Web Services (AWS). Ele foi projetado para oferecer alta performance, escalabilidade e durabilidade, sendo uma escolha popular para aplicativos e serviços que requerem um banco de dados confiável.

### Principais Características do Amazon Aurora

- **Compatibilidade com MySQL e PostgreSQL**: O Aurora é compatível com as versões do MySQL e PostgreSQL, o que facilita a migração de aplicativos existentes para o serviço.

- **Performance Superior**: Ele oferece uma performance excepcional devido à sua arquitetura de armazenamento distribuído e replicação robusta.

- **Replicação Contínua**: O Aurora oferece replicação contínua e automática para melhorar a disponibilidade e a recuperação de desastres.

- **Escalabilidade**: É possível redimensionar verticalmente (upscaling ou downscaling) o Amazon Aurora de acordo com as necessidades da aplicação.

- **Durabilidade**: Os dados são distribuídos em três zonas de disponibilidade da AWS para garantir alta durabilidade e recuperação de falhas.

- **Recuperação Automática de Falhas**: O Aurora monitora e recupera automaticamente de falhas de armazenamento físico.

- **Backup Contínuo**: Ele realiza backups contínuos e automatizados, permitindo a restauração do banco de dados para qualquer ponto no tempo.

- **Escalabilidade Horizontal**: Permite criar réplicas de leitura para distribuir a carga de leitura, melhorando o desempenho das consultas.

- **Endpoints Personalizáveis**: Fornece endpoints personalizáveis para leitura e gravação, permitindo direcionar consultas de leitura para réplicas de leitura.

- **Integração com RDS**: Pode ser gerenciado através do Amazon RDS (Relational Database Service), simplificando a administração do banco de dados.

### Arquitetura do Amazon Aurora

O Amazon Aurora utiliza uma arquitetura de banco de dados altamente distribuída e replicação de dados. Os dados são replicados para seis instâncias em três zonas de disponibilidade da AWS para garantir alta disponibilidade e durabilidade.

### Escalabilidade e Réplicas de Leitura

O Aurora oferece escalabilidade horizontal, permitindo que você crie réplicas de leitura para distribuir a carga de leitura e melhorar o desempenho das consultas. Você pode criar até 15 réplicas de leitura em um cluster do Aurora.

### Backup e Recuperação

O serviço realiza backups automáticos e contínuos do banco de dados. Isso permite que você restaure o banco de dados para qualquer ponto no tempo dentro do período de retenção configurado.

### Alta Disponibilidade

O Amazon Aurora foi projetado para alta disponibilidade, com a replicação de dados em três zonas de disponibilidade. Isso garante que o banco de dados continue funcionando mesmo em caso de falha em uma zona.

### Segurança

O Aurora oferece recursos avançados de segurança, incluindo criptografia de dados em repouso e em trânsito. Ele também integra-se ao AWS Identity and Access Management (IAM) para controle de acesso refinado.

---
## Atualizações & Serviços Adicionais (Foco de Prova)

### Aurora Serverless v2
- Escala granular (capacidade elástica automática ACU) praticamente em tempo real; ideal para workloads com picos imprevisíveis.

### Aurora Global Database
- Replicação cross-region <1s lag típico; para DR + leitura global.

### Aurora Backtrack
- Voltar cluster a ponto anterior em minutos (até 72h janela) sem restaurar snapshot completo.

### RDS Proxy
- Pool de conexões (MySQL/PostgreSQL/Aurora). Reduz overhead de connection storms (ex.: Lambda). Melhora failover.

### DynamoDB (Recursos Essenciais Exame)
| Recurso | Descrição | Uso |
|---------|-----------|-----|
| DAX | Cache in-memory fully managed | Reduz latência read (<ms) |
| Global Tables v2 | Replicação multi-região ativa | Low-latency global + DR |
| Streams | Fluxo de mudanças (near real-time) | Triggers Lambda / replicação custom |
| TTL | Expira itens automaticamente | Sessões / limpeza |
| PITR | Point-in-Time Recovery (últimas 35 dias) | Proteção contra deleção acidental |
| PartiQL | SQL-like para DynamoDB | Facilidade de consulta | 

### Consistência DynamoDB
- Read eventual (default) vs strongly consistent (na mesma região); Global Tables sempre eventual cross-region.

### Redshift (Novidades)
- RA3: separa compute x storage (managed storage). Multi-AZ com RA3 (2024+). 
- Redshift Serverless: paga por RPU hora; ideal workloads esporádicos.
- AQU (Advanced Query Accelerator) & Result cache.
- Data Sharing entre namespaces/clusters.

### Outros Serviços a Conhecer
| Serviço | Tipo | Caso de Uso |
|---------|------|-------------|
| Neptune | Grafo (Property + RDF/SPARQL) | Redes sociais, fraude |
| DocumentDB | Document (Mongo-compatible API) | Migração parcial de workloads Mongo |
| Keyspaces | Cassandra API | Workloads Cassandra gerenciado |
| MemoryDB for Redis | Redis com durabilidade multi-AZ | Sessões críticas / state store |
| Timestream | Time-series | IoT, métricas |
| QLDB | Ledger imutável criptograficamente | Rastreamento de transações (audit) |

### Caching Estratégico
- ElastiCache (Redis) para ranking, sessões, contadores atômicos.
- ElastiCache Memcached para sharding simples + cache de objeto.

### DR & Alta Disponibilidade – Padrões
| Necessidade | Padrão |
|-------------|--------|
| RPO≈0 & RTO baixo | Multi-AZ (RDS/Aurora) |
| Escala leitura sem impacto write | Read Replicas |
| DR cross-region rápido | Global Database (Aurora) / Global Tables (DynamoDB) |
| Recuperar dados deletados acidentalmente | PITR (DynamoDB) / Backups automáticos |

### Perguntas Típicas
1. Latência leitura <1ms em tabela DynamoDB com picos → DAX.
2. Failover automático sem perda de dados RDS → Multi-AZ (não confundir com Read Replica).
3. Escalar somente leitura para relatórios → Read Replicas.
4. Replicação global ativa para NoSQL → DynamoDB Global Tables.
5. Reduzir overhead de conexões por milhares de Lambdas → RDS Proxy.

---
## Panorama de Serviços e Capacidades (Resumo 2025)
Valores típicos públicos. Podem variar por região/atualização. Sempre validar na documentação oficial.

| Serviço | Categoria | Capacidade / Escala Principal | Limites-Chave | Observações |
|---------|-----------|-------------------------------|---------------|-------------|
| RDS (MySQL/PostgreSQL/MariaDB/Oracle/SQL Server) | Relacional OLTP | Até 128 TiB por instância (gp3/io1) | Até 15 Read Replicas (varia engine) | Multi-AZ sincrono; IOPS até 256K+ (gp3/io1) |
| Amazon Aurora (MySQL/PostgreSQL compat.) | Relacional OLTP | Storage elástico até 128 TiB (cluster) | 15 réplicas, 5 regiões secundárias (Global DB) | Backtrack (até 72h), Serverless v2 (ACUs) |
| Amazon Aurora Global Database | Relacional Global | 1 primária + até 5 regiões secundárias | Lag <1s típico | Failover cross-region (DR) |
| Amazon Redshift (RA3) | Data Warehouse | Vários PB comprimidos (Managed Storage) | Concurrency scaling sob demanda | RA3 com Multi-AZ / Serverless para cargas esporádicas |
| Redshift Serverless | Data Warehouse | Escala automática PB | Armazenamento gerenciado | Cobra por RPU-hora |
| DynamoDB | Chave-Valor / Documento | Tabela ilimitada (particiona) | Item ≤400 KB; 20 GSI (soft, pode mudar); LSI ≤5 | Global Tables multi-região ativa |
| DynamoDB DAX | Cache DynamoDB | Escala horizontal clusters | Memória por nó (dependência instância) | Latência micro/milisegundo |
| Amazon Keyspaces | Wide Column (Cassandra) | Tabela ilimitada | Linha ≤1 MB | On-demand ou provisionado |
| Amazon DocumentDB | Documento (Mongo API) | Storage elástico até 128 TiB | Documento ≤16 MB | Até 15 réplicas |
| Amazon Neptune | Grafo | Até 128 TiB storage | 15 réplicas leitura | Suporta Gremlin / SPARQL / openCypher |
| Amazon QLDB | Ledger Imutável | Sem limite prático | Tamanho transação/documento prático (JSON ≤ ~128 KB recomendado) | Histórico criptograficamente verificável |
| Amazon Timestream | Time Series | Escala automática (multi-tier) | Sem tamanho fixo; linhas removidas conforme política | Memory Store + Magnetic Store |
| ElastiCache Redis (Cluster Mode Enabled) | In-Memory | Até 500 shards; nó ~ <635 GiB | 5 réplicas por shard | Centenas de TiB agregados |
| ElastiCache Memcached | In-Memory | Até 100 nós | Tamanho depende instância | Sem replicação nativa (sharding) |
| MemoryDB for Redis | In-Memory Durável | Até 500 shards | Shard ~500+ GiB (classe grande) | Multi-AZ com persistência |
| OpenSearch Service | Busca / Analytics | Data node EBS até 24 TiB (gp3/io1) * nós | Documento JSON ≤ ~100 MB recomendado | PB lógicos via sharding |
| OpenSearch Vector Search | Similaridade | Mesmo limites base | Vetor dimensão (dep. versão) | Embeddings p/ IA busca semântica |
| Amazon Neptune Analytics (se habilitado) | Grafo analítico | Processa datasets multi-TiB | - | Complementar a Neptune core |
| Amazon RDS Proxy | Conexões | Proxy escalável | Limites conexões por proxy (ajustáveis) | Reduz storms de conexão |
| Amazon Glue Data Catalog (metastore) | Catálogo | Metadados milhões de objetos | Tabelas/partições quotas eleváveis | Não é DB de aplicação |

### Limites Específicos Importantes
- DynamoDB: Partição física ~10 GB antes de split; throughput por partição adaptativo; hot partition é risco de design de chave.
- Aurora: Segmenta páginas de 10 GB; auto-expand; latência de replicação intra-cluster milissegundos.
- Redshift: Result cache pode evitar scan; Spectrum lê dados no S3 sem mover para cluster.
- OpenSearch: Ajustar shard count (excesso → overhead; falta → impossibilita escala futura).
- MemoryDB vs ElastiCache Redis: MemoryDB prioriza durabilidade (multi-AZ write-ahead log), maior latência write que Redis puro.

### Itens/Objetos Máximos (Referência Rápida)
| Item | Limite |
|------|--------|
| DynamoDB Item | 400 KB (atributos + nomes + overhead) |
| DocumentDB Documento | 16 MB (limite Mongo compat.) |
| S3 Objeto (para integração de lakes) | 5 TB |
| ElastiCache Key (Redis) | 512 MB valor (não recomendado próximo disso) |

### Seleção Rápida (Heurística)
| Requisito | Serviço Primário |
|-----------|-----------------|
| Alta taxa escrita key-value, escala imprevisível | DynamoDB |
| Relacional escalável com failover rápido | Aurora |
| DW analítico petabyte | Redshift |
| Time Series IoT | Timestream |
| Busca texto + agregações | OpenSearch |
| Grafo relacionamento | Neptune |
| Ledger auditável | QLDB |
| Cache sub-ms / ranking | ElastiCache Redis |
| Cassandra compat. gerenciado | Keyspaces |

### Perguntas de Exame Ligadas a Capacidade
1. Necessário petabytes analíticos + SQL → Redshift (RA3/Serverless) não Aurora.
2. Latência leitura micro/milisegundos sub-ms para chave → DynamoDB + DAX.
3. Crescimento imprevisível sem pré-provisionar partições → DynamoDB on-demand.
4. Replicação global ativa write em múltiplas regiões relacional → Aurora Global (não RDS tradicional).
5. Cache distribuído escalável com partição manual → ElastiCache Memcached.

> Dica: Quando a pergunta enfatiza "tamanho ilimitado / escala automática sem administração de partição" tende a ser DynamoDB / Keyspaces / Timestream; quando enfatiza SQL transacional + alta disponibilidade → Aurora/RDS Multi-AZ.

---
## Próximos Passos
Se quiser, podemos gerar um quadro comparativo de custos relativos ou adicionar fluxogramas de decisão (ex.: Aurora vs DynamoDB vs Redshift) em outro arquivo.


