### O que é Amazon DynamoDB

O Amazon DynamoDB é um serviço de banco de dados NoSQL totalmente gerenciado que oferece desempenho rápido e previsível com escalabilidade perfeita. Ele é um banco de dados de chave-valor e de documentos que pode lidar com mais de 10 trilhões de solicitações por dia e suportar picos de mais de 20 milhões de solicitações por segundo. O DynamoDB tira de você o fardo de operar e escalar um banco de dados distribuído.

### Como funciona o Amazon DynamoDB

1.  **Criação de uma Tabela:** Você começa criando uma tabela. Ao criar a tabela, você especifica uma chave primária, que identifica exclusivamente cada item na tabela.
2.  **Chave Primária:** A chave primária pode ser de dois tipos:
    *   **Chave de Partição (Partition Key):** Uma chave simples, composta por um atributo. O DynamoDB usa o valor da chave de partição como entrada para uma função de hash interna para determinar a partição (armazenamento físico) onde o item será armazenado.
    *   **Chave de Partição e Classificação (Partition Key and Sort Key):** Uma chave composta. Itens com a mesma chave de partição são armazenados juntos, ordenados pela chave de classificação.
3.  **Inserção e Consulta de Dados:** Você insere, atualiza e exclui itens na tabela usando a chave primária. As consultas são extremamente rápidas quando usam a chave primária.
4.  **Escalonamento Automático:** O DynamoDB monitora o tráfego e particiona automaticamente os dados e o tráfego por vários servidores para atender à capacidade de throughput solicitada. Ele escala horizontalmente sem tempo de inatividade.
5.  **Modos de Capacidade:**
    *   **Provisionado (Provisioned):** Você especifica o número de leituras e escritas por segundo que sua aplicação precisa. Ideal para cargas de trabalho previsíveis.
    *   **Sob Demanda (On-Demand):** O DynamoDB adapta-se instantaneamente às suas cargas de trabalho à medida que aumentam ou diminuem. Ideal para cargas de trabalho imprevisíveis.

### Recursos Principais

*   **DynamoDB Accelerator (DAX):** Um cache na memória totalmente gerenciado e altamente disponível para o DynamoDB que oferece um desempenho até 10 vezes mais rápido, de milissegundos para microssegundos.
*   **Global Tables (Tabelas Globais):** Permitem que você crie um banco de dados distribuído globalmente e totalmente replicado, permitindo acesso de baixa latência para usuários em todo o mundo e resiliência contra falhas regionais.
*   **Streams:** Captura uma sequência ordenada de modificações em nível de item em qualquer tabela do DynamoDB. Você pode usar streams com o AWS Lambda para criar gatilhos (triggers).
*   **Índices Secundários (Secondary Indexes):** Permitem que você consulte os dados na tabela usando um atributo alternativo à chave primária.

### Benefícios do DynamoDB

*   **Desempenho em Escala:** Oferece latência de milissegundos de um dígito em qualquer escala.
*   **Totalmente Gerenciado (Serverless):** Não há servidores para provisionar, corrigir ou gerenciar, nem software para instalar, manter ou operar.
*   **Alta Disponibilidade e Durabilidade:** Os dados são replicados automaticamente em três Zonas de Disponibilidade em uma região.
*   **Segurança:** Criptografa todos os dados em repouso e oferece controle de acesso granular com o IAM.
*   **Flexibilidade:** Sendo um banco de dados NoSQL, ele não exige um esquema fixo, permitindo que você armazene dados complexos e hierárquicos.
