### O que é AWS DMS (Database Migration Service)

O AWS Database Migration Service (DMS) é um serviço de nuvem que facilita a migração de bancos de dados relacionais, data warehouses, bancos de dados NoSQL e outros armazenamentos de dados para a AWS de forma rápida e segura. Você pode usar o DMS para migrar seus dados para a nuvem da AWS, entre instâncias on-premises (por meio de uma conexão da AWS) ou entre uma combinação de nuvem e bancos de dados on-premises.

### Como funciona o AWS DMS

O DMS funciona conectando-se a fontes de dados de origem e de destino, lendo os dados da origem e gravando-os no destino. O processo é gerenciado por uma **tarefa de replicação** que é executada em uma **instância de replicação**.

1.  **Endpoints de Origem e Destino (Source and Target Endpoints):** Você cria endpoints que contêm as informações de conexão para seus bancos de dados de origem e de destino. O DMS usa esses endpoints para se conectar aos armazenamentos de dados.
2.  **Instância de Replicação (Replication Instance):** É uma instância EC2 gerenciada que hospeda uma ou mais tarefas de replicação. A instância de replicação fica em sua VPC e precisa de conectividade de rede com seus endpoints de origem e destino. Ela realiza a movimentação de dados entre a origem e o destino.
3.  **Tarefa de Migração (Migration Task):** A tarefa de migração é onde você define o que e como migrar. Ela conecta os endpoints de origem e destino e especifica as configurações da migração. Existem três tipos principais de tarefas de migração:
    *   **Migração de Carga Completa (Full Load):** Migra todos os dados existentes do banco de dados de origem para o de destino.
    *   **Captura de Dados de Alteração (Change Data Capture - CDC):** Também conhecida como replicação contínua. Após a carga completa, o DMS captura as alterações em andamento no banco de dados de origem (usando os logs de transação do banco de dados) e as aplica ao destino. Isso mantém os bancos de dados de origem e destino em sincronia.
    *   **Carga Completa e CDC:** A abordagem mais comum. O DMS primeiro realiza uma carga completa dos dados e, em seguida, muda para o modo CDC para replicar as alterações contínuas.

### Migração com Tempo de Inatividade Mínimo

O recurso de CDC é o que permite migrações de banco de dados com tempo de inatividade mínimo. O fluxo de trabalho típico é:
1.  Iniciar uma tarefa de "Carga Completa e CDC".
2.  O DMS copia os dados existentes (carga completa).
3.  Enquanto a carga completa está em andamento, o DMS começa a capturar e armazenar em cache as alterações que ocorrem na origem.
4.  Após a conclusão da carga completa, o DMS aplica as alterações em cache ao destino e continua a replicar as alterações em tempo real.
5.  Quando os bancos de dados de origem e destino estiverem sincronizados, você pode redirecionar o tráfego de sua aplicação para o banco de dados de destino e encerrar o banco de dados de origem, resultando em um tempo de inatividade de apenas alguns minutos.

### AWS Schema Conversion Tool (SCT)

O DMS migra os dados, mas **não migra o esquema** do banco de dados (como tabelas, chaves primárias, chaves estrangeiras, etc.). Para migrações **homogêneas** (por exemplo, de MySQL para MySQL), a migração do esquema é geralmente simples.

Para migrações **heterogêneas** (por exemplo, de Oracle para PostgreSQL), você precisa do **AWS Schema Conversion Tool (SCT)**.
*   **O que faz:** O SCT analisa o esquema do banco de dados de origem e o converte automaticamente para um formato compatível com o banco de dados de destino. Ele também identifica e destaca qualquer código (como procedimentos armazenados ou funções) que não pode ser convertido automaticamente, fornecendo dicas sobre como adaptá-lo manualmente.
*   **Como funciona:** Você executa o SCT em sua máquina local ou em uma instância EC2. Ele se conecta às suas fontes de dados, gera um relatório de avaliação e cria os scripts SQL para criar o esquema no banco de dados de destino.

### Benefícios do DMS

*   **Simples de Usar:** O DMS é fácil de configurar e gerencia todas as complexidades do processo de migração.
*   **Tempo de Inatividade Mínimo:** A replicação contínua de dados permite que o banco de dados de origem permaneça operacional durante a migração.
*   **Suporte a Vários Bancos de Dados:** Suporta uma ampla variedade de fontes e destinos de banco de dados populares.
*   **Confiável e Resiliente:** O DMS monitora continuamente a instância de replicação e os bancos de dados e reinicia automaticamente em caso de falha.
*   **Baixo Custo:** Você paga apenas pela instância de replicação e por qualquer armazenamento de log adicional.
