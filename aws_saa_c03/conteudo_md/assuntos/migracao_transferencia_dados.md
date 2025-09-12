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
    -   **Amazon AppFlow**

A seguir, detalhamos cada um desses serviços.

## 1. AWS Storage Gateway

O AWS Storage Gateway é um serviço híbrido que conecta seus ambientes de aplicações on-premises ao armazenamento na nuvem da AWS. Ele fornece uma ponte segura e de baixa latência entre seus sistemas locais e os serviços de armazenamento da AWS, como S3, EBS e Glacier.

Existem três tipos principais de gateways:

-   **Amazon S3 File Gateway:**
    -   **O que é:** Fornece uma interface de arquivos para o Amazon S3, permitindo que você armazene e acesse objetos no S3 como se fossem arquivos em um compartilhamento de rede local, utilizando protocolos padrão como NFS (Network File System) e SMB (Server Message Block).
    -   **Como Funciona:**
        1.  **VM Local:** Você implanta uma máquina virtual (VM) do Storage Gateway em seu data center on-premises.
        2.  **Compartilhamento de Arquivos:** A VM apresenta um ou mais compartilhamentos de arquivos para seus servidores e aplicações locais.
        3.  **Mapeamento para o S3:** Cada compartilhamento de arquivos é mapeado para um bucket S3 específico.
        4.  **Acesso Transparente:** Suas aplicações gravam e leem arquivos nesse compartilhamento como fariam em qualquer servidor de arquivos local. O gateway cuida de transferir esses arquivos de forma assíncrona para o S3 como objetos.
        5.  **Cache Local:** O gateway mantém um cache local dos arquivos acessados recentemente, garantindo acesso de baixa latência para os dados mais "quentes".
    -   **Principais Casos de Uso:**
        *   **Migração de Dados para a Nuvem:** Simplifica a migração de dados de servidores de arquivos (file servers) para o Amazon S3.
        *   **Backup e Arquivamento:** Permite que ferramentas de backup que operam com compartilhamentos de arquivos façam backup diretamente na nuvem (S3).
        *   **Workflows Híbridos:** Facilita o processamento de dados na nuvem. Dados gerados on-premises podem ser gravados no File Gateway, aparecendo quase que instantaneamente no S3 para serem processados por serviços da AWS como EMR, Athena ou SageMaker.

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

---

## 5. Amazon AppFlow

O Amazon AppFlow é um serviço de integração totalmente gerenciado que permite transferir dados de forma segura entre aplicações SaaS (Software as a Service), como Salesforce, Slack, Zendesk, e serviços da AWS, como Amazon S3 e Amazon Redshift.

-   **Como funciona:** Com apenas alguns cliques, você pode configurar "fluxos" para mover dados entre uma fonte e um destino. O AppFlow cuida da conectividade, autenticação, mapeamento de dados e transformações leves.
-   **Principais Características:**
    -   **Sem Código (No-Code):** A interface é totalmente visual, permitindo que usuários de negócios configurem integrações de dados sem escrever código.
    -   **Transferência Privada:** Pode ser configurado para que os dados fluam pela rede privada da AWS, sem passar pela internet pública.
-   **Comparação com AWS Glue:**
    -   **AppFlow:** É ideal para integrações ponto a ponto com aplicações SaaS, focada em simplicidade e rapidez, sem a necessidade de código.
    -   **AWS Glue:** É uma ferramenta de ETL poderosa e flexível, projetada para transformações de dados em grande escala, geralmente em um contexto de data lake ou data warehouse. Requer mais conhecimento técnico.