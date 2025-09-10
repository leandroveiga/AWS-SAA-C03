# Bancos de Dados na AWS

A AWS oferece um portfólio abrangente de serviços de banco de dados, projetados para diferentes casos de uso, desde bancos de dados relacionais tradicionais até soluções NoSQL em escala de internet.

---

## Amazon RDS (Relational Database Service)

O **RDS** é um serviço gerenciado que facilita a configuração, operação e escalabilidade de um banco de dados relacional na nuvem. Ele automatiza tarefas demoradas de administração, como provisionamento de hardware, configuração de banco de dados, patches e backups.

-   **Motores Suportados:** Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle e Microsoft SQL Server.
-   **Principal Vantagem:** Permite que você se concentre em suas aplicações, enquanto a AWS cuida da administração do banco de dados.

### Alta Disponibilidade e Escalabilidade no RDS

#### Multi-AZ
-   **Propósito:** Alta Disponibilidade e Recuperação de Desastres (DR).
-   **Como Funciona:** O RDS cria e mantém uma réplica **síncrona** e em modo de espera (standby) do seu banco de dados principal em uma Zona de Disponibilidade (AZ) diferente.
-   **Failover:** Se o banco de dados principal falhar, o RDS realiza um failover automático para a réplica em espera, minimizando o tempo de inatividade. A aplicação se reconecta usando o mesmo endpoint de conexão.
-   **Importante:** A réplica standby **não pode** ser usada para servir tráfego de leitura. Seu único propósito é estar pronta para assumir em caso de falha.

<p align="center">
    <img src="https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/images/con-multi-AZ.png" alt="Implantação RDS Multi-AZ com instância principal e standby" width="520" />
    <br/>
    <em>Fonte: Documentação oficial Amazon RDS</em>
</p>

#### Read Replicas (Réplicas de Leitura)
-   **Propósito:** Escalabilidade de Leitura (melhorar a performance).
-   **Como Funciona:** O RDS cria uma ou mais cópias **assíncronas** do banco de dados principal.
-   **Uso:** Você pode direcionar as consultas de leitura (operações `SELECT`) da sua aplicação para as Read Replicas, reduzindo a carga no banco de dados principal e melhorando o desempenho geral.
-   **Flexibilidade:** Read Replicas podem ser criadas em diferentes AZs ou até mesmo em diferentes Regiões (Cross-Region Read Replicas), o que também ajuda a reduzir a latência para usuários globais.
-   **Promoção:** Uma Read Replica pode ser "promovida" para se tornar um banco de dados principal independente, se necessário.

<p align="center">
    <img src="https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/images/read-replica-cross-region.png" alt="RDS Read Replicas incluindo replicação cross-region" width="560" />
    <br/>
    <em>Fonte: Documentação oficial Amazon RDS</em>
</p>

| Recurso | Propósito Principal | Replicação | Failover |
| :--- | :--- | :--- | :--- |
| **Multi-AZ** | Alta Disponibilidade / DR | Síncrona | Automático |
| **Read Replica** | Escalabilidade de Leitura | Assíncrona | Manual |

---

## Amazon Aurora

O **Aurora** é um banco de dados relacional compatível com MySQL e PostgreSQL, construído para a nuvem. Ele combina a performance e a disponibilidade de bancos de dados comerciais de ponta com a simplicidade e a economia de bancos de dados de código aberto.

-   **Performance:** Até 5x mais rápido que o MySQL padrão e 3x mais rápido que o PostgreSQL padrão.
-   **Arquitetura:** Separa a computação do armazenamento. O volume de armazenamento é distribuído, tolerante a falhas e auto-reparável, replicando 6 cópias dos seus dados em 3 Zonas de Disponibilidade.
-   **Alta Disponibilidade:** O failover para uma réplica Aurora geralmente leva menos de 30 segundos.
-   **Endpoints:** O Aurora fornece um **endpoint de cluster (writer)** para o nó principal e um **endpoint de leitor (reader)** que faz o balanceamento de carga entre todas as réplicas de leitura.

---

## Amazon DynamoDB

O **DynamoDB** é um banco de dados NoSQL de chave-valor e de documentos, totalmente gerenciado, que oferece performance de milissegundos de um dígito em qualquer escala.

-   **Serverless:** Não há servidores para provisionar, aplicar patches ou gerenciar.
-   **Escalabilidade:** Altamente escalável, com capacidade de leitura e gravação que pode ser ajustada automaticamente (On-Demand) ou provisionada.
-   **Consistência de Leitura:**
    -   **Eventually Consistent Reads (Padrão):** A leitura pode não refletir os resultados de uma gravação concluída recentemente. Oferece a maior performance de leitura.
    -   **Strongly Consistent Reads:** A leitura retorna um resultado que reflete todas as gravações que receberam uma resposta bem-sucedida antes da leitura. Garante que você sempre leia o dado mais recente, com uma latência ligeiramente maior.
-   **Global Tables:** Permite criar um banco de dados totalmente replicado e multi-ativo em múltiplas regiões da AWS, ideal para aplicações globais de baixa latência.

---

## Amazon ElastiCache

O **ElastiCache** é um serviço web que facilita a implantação, a operação e a escalabilidade de um cache na memória na nuvem. Ele melhora a performance de aplicações web, permitindo que você recupere informações de caches na memória rápidos e gerenciados, em vez de depender inteiramente de bancos de dados baseados em disco, que são mais lentos.

-   **Motores Suportados:**
    -   **Redis:** Um armazenamento de estrutura de dados na memória, rápido e de código aberto, usado como banco de dados, cache e message broker. Suporta estruturas de dados mais complexas (listas, hashes, sets) e recursos como replicação e persistência.
    -   **Memcached:** Um sistema de cache de objetos na memória distribuído, de alta performance. É mais simples que o Redis, ideal para armazenar em cache objetos simples de chave-valor.

---

## Amazon Redshift

O **Redshift** é um serviço de data warehouse em escala de petabytes, rápido e totalmente gerenciado na nuvem.

-   **Propósito:** Análise de dados (OLAP - Online Analytical Processing), não para processamento de transações (OLTP).
-   **Arquitetura:** Utiliza armazenamento colunar e processamento massivamente paralelo (MPP) para executar consultas complexas em grandes quantidades de dados de forma extremamente rápida.
-   **Caso de Uso:** Business intelligence, análise de big data e relatórios.
-   **Disponibilidade:** O Redshift é um serviço de **nó único** ou **cluster multi-nó**, mas opera em uma **única Zona de Disponibilidade**. Para alta disponibilidade, você precisa configurar snapshots e replicação para outra região.


