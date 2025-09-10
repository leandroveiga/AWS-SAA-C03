### O que é Amazon SQS (Simple Queue Service)

O Amazon Simple Queue Service (SQS) é um serviço de enfileiramento de mensagens totalmente gerenciado que permite desacoplar e escalar microsserviços, sistemas distribuídos e aplicações sem servidor. O SQS elimina a complexidade e a sobrecarga associadas ao gerenciamento e operação de middleware orientado a mensagens, e permite que os desenvolvedores se concentrem em seu trabalho de diferenciação.

### Como funciona o Amazon SQS

1.  **Componentes Produtor e Consumidor:** Um sistema que usa SQS tem três partes principais:
    *   **Produtores (Producers):** Componentes que enviam mensagens para uma fila.
    *   **Fila SQS (Queue):** Armazena as mensagens. As mensagens são mantidas na fila até que um consumidor as processe e exclua.
    *   **Consumidores (Consumers):** Componentes que recuperam mensagens da fila, as processam e depois as excluem.
2.  **Desacoplamento:** O produtor e o consumidor não interagem diretamente. O produtor simplesmente coloca uma mensagem na fila. O consumidor pega uma mensagem da fila para processar quando estiver pronto. Eles não precisam estar online ao mesmo tempo. Isso cria um sistema desacoplado e resiliente.
3.  **Processo de Mensagem:**
    *   Um produtor envia uma mensagem para a fila.
    *   Um consumidor solicita mensagens da fila (polling).
    *   O SQS torna a mensagem "invisível" por um período de tempo configurável, chamado de **Visibility Timeout (Tempo de Visibilidade)**. Isso evita que outros consumidores processem a mesma mensagem.
    *   O consumidor processa a mensagem.
    *   Após o processamento bem-sucedido, o consumidor envia um comando para **excluir a mensagem** da fila.
    *   Se o consumidor falhar e não excluir a mensagem antes que o tempo de visibilidade expire, a mensagem se torna visível novamente na fila para que outro consumidor possa processá-la.

### Tipos de Fila

#### 1. Filas Padrão (Standard Queues)
*   **O que são:** O tipo de fila padrão.
*   **Características:**
    *   **Throughput Ilimitado:** Oferecem throughput quase ilimitado de transações por segundo.
    *   **Entrega "At-Least-Once" (Pelo Menos Uma Vez):** Uma mensagem é entregue pelo menos uma vez, mas ocasionalmente (em raras circunstâncias) mais de uma cópia de uma mensagem pode ser entregue.
    *   **Melhor Esforço de Ordenação (Best-Effort Ordering):** Ocasionalmente, as mensagens podem ser entregues em uma ordem diferente daquela em que foram enviadas.
*   **Caso de uso:** Ideal para cargas de trabalho que podem processar mensagens fora de ordem e lidar com mensagens duplicadas, como processamento em lote, transcodificação de mídia ou envio de e-mails.

#### 2. Filas FIFO (First-In, First-Out)
*   **O que são:** Projetadas para garantir que a ordem em que as mensagens são enviadas e recebidas seja estritamente preservada.
*   **Características:**
    *   **Processamento "Exactly-Once" (Exatamente Uma Vez):** As duplicatas não são introduzidas na fila.
    *   **Ordenação First-In, First-Out:** A ordem das mensagens é garantida.
    *   **Throughput Limitado:** Suportam até 3.000 mensagens por segundo com processamento em lote, ou até 300 por segundo sem.
*   **Caso de uso:** Essencial para aplicações onde a ordem das operações e eventos é crítica, como comandos bancários, registros de votação ou atualização de inventário.

### Benefícios do SQS

*   **Desacoplamento e Resiliência:** Aumenta a resiliência do sistema. Se um componente falhar, as mensagens podem permanecer na fila para serem processadas mais tarde.
*   **Escalabilidade:** Permite que os componentes do produtor e do consumidor escalem de forma independente.
*   **Totalmente Gerenciado:** A AWS gerencia toda a infraestrutura subjacente.
*   **Segurança:** Oferece criptografia em trânsito e em repouso.
