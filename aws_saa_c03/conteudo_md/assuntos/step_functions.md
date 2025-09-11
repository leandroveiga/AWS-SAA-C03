# Orquestração Serverless com AWS Step Functions

O **AWS Step Functions** é um serviço de orquestração serverless que permite sequenciar funções AWS Lambda e múltiplos serviços da AWS em fluxos de trabalho de negócios críticos. Com o Step Functions, você pode criar "máquinas de estado" visuais que são fáceis de entender e depurar.

## Conceito Principal: Máquinas de Estado

Sua aplicação é definida como uma máquina de estado, onde cada passo é um "Estado".

-   **Task:** Um estado que representa uma única unidade de trabalho realizada por outro serviço da AWS (ex: invocar uma função Lambda, iniciar um job no AWS Batch, inserir um item no DynamoDB).
-   **Choice:** Adiciona lógica de ramificação (if/then/else) ao seu fluxo de trabalho.
-   **Parallel:** Permite executar ramificações de execução em paralelo.
-   **Map:** Permite executar um conjunto de passos para cada item de um array de entrada (processamento dinâmico em paralelo).
-   **Wait:** Adiciona um atraso ao fluxo de trabalho por um tempo especificado.

## Tipos de Fluxo de Trabalho

-   **Standard (Padrão):** Ideal para fluxos de trabalho de longa duração (até 1 ano), duráveis e auditáveis. A execução é "exactly-once".
-   **Express (Expresso):** Ideal para fluxos de trabalho de alto volume e curta duração (até 5 minutos), como processamento de eventos de streaming de dados ou backends de API. A execução é "at-least-once".

## Por que usar Step Functions em vez de encadear Lambdas?

-   **Visibilidade e Depuração:** Fornece um console visual que mostra o histórico de execução de cada passo, facilitando a identificação de falhas.
-   **Tratamento de Erros:** Possui lógica de `Retry` (tentativa) e `Catch` (captura) incorporada, simplificando o tratamento de falhas.
-   **Estado:** Gerencia o estado entre os passos. Você não precisa passar o estado manualmente ou armazená-lo em um banco de dados.
-   **Orquestração vs. Coreografia:** Enquanto SQS/SNS promovem a coreografia (serviços reagem a eventos sem um controlador central), o Step Functions permite a orquestração (um controlador central dita o fluxo).
