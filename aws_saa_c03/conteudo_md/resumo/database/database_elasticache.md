### O que é Amazon ElastiCache

O Amazon ElastiCache é um serviço da web totalmente gerenciado que facilita a implantação, operação e escalonamento de um cache na memória na nuvem. Ele melhora o desempenho de aplicações da web, permitindo que você recupere informações de caches na memória rápidos e gerenciados, em vez de depender inteiramente de bancos de dados baseados em disco, que são mais lentos.

### Como funciona o Amazon ElastiCache

1.  **Seleção do Mecanismo de Cache:** Você escolhe um dos dois mecanismos de cache de código aberto mais populares:
    *   **Redis:** Um armazenamento de estrutura de dados na memória, rápido e de código aberto, usado como banco de dados, cache e message broker. Suporta estruturas de dados mais complexas, como strings, hashes, listas, conjuntos, conjuntos ordenados, e oferece recursos como replicação, alta disponibilidade (Multi-AZ) e particionamento de dados (clustering).
    *   **Memcached:** Um sistema de cache de objetos de memória distribuída de alto desempenho, destinado a acelerar aplicações da web dinâmicas, aliviando a carga do banco de dados. É mais simples, projetado para escalabilidade e ideal para armazenar objetos simples de chave-valor.
2.  **Lançamento de um Cluster:** Você lança um cluster de cache, especificando o tipo de nó (CPU/memória) e o número de nós. O ElastiCache cuida do provisionamento, patching e gerenciamento dos nós.
3.  **Integração com a Aplicação:** Sua aplicação se conecta ao cluster ElastiCache usando o endpoint fornecido. A lógica da aplicação é modificada para primeiro verificar se os dados existem no cache.
    *   **Cache Hit (Acerto no Cache):** Se os dados estiverem no cache, a aplicação os lê diretamente, o que é muito rápido.
    *   **Cache Miss (Falta no Cache):** Se os dados não estiverem no cache, a aplicação os busca no banco de dados principal (por exemplo, RDS ou DynamoDB), armazena uma cópia no cache e, em seguida, os utiliza.
4.  **Estratégias de Cache:**
    *   **Lazy Loading (Carregamento Lento):** Carrega dados no cache apenas quando necessário (cache miss). É simples de implementar.
    *   **Write-Through (Escrita Direta):** Adiciona ou atualiza dados no cache sempre que os dados são gravados no banco de dados. Mantém o cache consistente, mas adiciona latência à escrita.

### Benefícios do ElastiCache

*   **Desempenho Extremo:** O cache na memória reduz drasticamente a latência, melhorando o desempenho geral e a capacidade de resposta da aplicação.
*   **Totalmente Gerenciado:** Automatiza tarefas comuns de gerenciamento, como configuração, patching de software, monitoramento e recuperação de falhas.
*   **Escalabilidade:** Permite escalar facilmente a capacidade de cache para cima ou para baixo para atender à demanda da aplicação.
*   **Alta Disponibilidade (com Redis):** O ElastiCache para Redis oferece suporte a Multi-AZ com failover automático, aumentando a confiabilidade da sua camada de cache.
*   **Redução da Carga do Banco de Dados:** Ao servir solicitações de leitura a partir do cache, ele reduz a carga sobre o banco de dados principal, o que pode diminuir os custos e evitar gargalos de desempenho.
