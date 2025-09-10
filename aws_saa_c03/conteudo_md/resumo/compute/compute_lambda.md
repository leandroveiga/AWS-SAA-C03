### O que é AWS Lambda

O AWS Lambda é um serviço de computação "serverless" (sem servidor) que executa seu código em resposta a eventos e gerencia automaticamente os recursos de computação subjacentes para você. Com o Lambda, você pode executar código para praticamente qualquer tipo de aplicação ou serviço de backend sem provisionar ou gerenciar servidores. Você paga apenas pelo tempo de computação que consome.

### Como funciona o AWS Lambda

1.  **Upload do Código:** Você escreve seu código (em linguagens como Python, Node.js, Java, etc.) e o empacota em uma função Lambda. O código deve ser projetado para ser executado de forma stateless.
2.  **Configuração do Gatilho (Trigger):** Você configura um gatilho que fará com que sua função seja executada. Gatilhos podem ser eventos de outros serviços da AWS (como um novo arquivo no S3, uma atualização em uma tabela DynamoDB, uma chamada de API Gateway) ou chamadas diretas via SDK.
3.  **Execução da Função:** Quando o evento de gatilho ocorre, o Lambda executa sua função em um ambiente de computação de alta disponibilidade. Ele lida com todo o provisionamento e gerenciamento de capacidade, CPU, memória e outros recursos.
4.  **Escalonamento Automático:** Se vários eventos ocorrerem simultaneamente, o Lambda simplesmente executa várias instâncias da sua função em paralelo, uma para cada evento. Ele escala de forma precisa e automática com o volume de solicitações.
5.  **Pagamento por Uso:** O faturamento é baseado no número de solicitações para suas funções e na duração (o tempo que leva para o seu código executar), medido em milissegundos.

### Casos de Uso Principais

*   **Processamento de Dados em Tempo Real:** Processar uploads de arquivos no S3, como redimensionar imagens ou transcodificar vídeos.
*   **Backends para Aplicações Web e Móveis:** Criar APIs RESTful usando o Amazon API Gateway para acionar funções Lambda que leem e escrevem em bancos de dados como o DynamoDB.
*   **Automação de Tarefas de TI:** Agendar funções para realizar tarefas de manutenção, como criar backups, verificar recursos ociosos ou gerar relatórios.
*   **Processamento de Streams:** Analisar dados de streaming em tempo real de serviços como o Amazon Kinesis.

### Benefícios do Lambda

*   **Sem Gerenciamento de Servidores:** Você nunca precisa se preocupar com o provisionamento, patching ou gerenciamento de servidores.
*   **Escalonamento Contínuo:** Escala automaticamente sua aplicação executando o código em resposta a cada gatilho.
*   **Custo-Benefício (Pagamento por Uso):** Você paga apenas pelo tempo de computação que consome, em incrementos de milissegundos, em vez de pagar por servidores ociosos.
*   **Velocidade de Desenvolvimento:** Permite focar na escrita da lógica de negócio em vez de gerenciar a infraestrutura, acelerando o ciclo de desenvolvimento.
