# Contêineres na AWS

Contêineres se tornaram o padrão para o desenvolvimento e implantação de aplicações modernas, especialmente microserviços. Eles empacotam o código de uma aplicação e todas as suas dependências em uma única imagem, garantindo que ela seja executada de forma consistente em qualquer ambiente. A AWS oferece um conjunto completo de serviços para executar e gerenciar cargas de trabalho em contêineres.

## 1. Amazon Elastic Container Registry (ECR)

O Amazon ECR é um registro de imagens de contêiner Docker totalmente gerenciado. Ele serve para armazenar, gerenciar, compartilhar e implantar suas imagens de contêiner.

-   **Funcionalidade Principal:** É o equivalente da AWS ao Docker Hub, mas privado, seguro e integrado ao ecossistema da AWS (especialmente IAM, ECS e EKS).
-   **Principais Características:**
    -   **Segurança:** As imagens são armazenadas em buckets S3 gerenciados pela AWS e criptografadas em repouso. A integração com o IAM permite um controle de acesso granular (quem pode enviar ou baixar imagens).
    -   **Análise de Vulnerabilidades:** O ECR pode escanear automaticamente suas imagens em busca de vulnerabilidades de software conhecidas.
    -   **Ciclo de Vida de Imagens:** Você pode definir políticas para limpar imagens antigas ou não utilizadas, ajudando a gerenciar os custos de armazenamento.

## 2. Orquestração de Contêineres

Depois que uma imagem de contêiner é armazenada no ECR, você precisa de um orquestrador para implantar, gerenciar e escalar os contêineres em execução. A AWS oferece duas opções principais: ECS e EKS.

| Característica | Amazon ECS (Elastic Container Service) | Amazon EKS (Elastic Kubernetes Service) |
| :--- | :--- | :--- |
| **Descrição** | Orquestrador de contêineres proprietário da AWS, totalmente gerenciado. | Serviço Kubernetes gerenciado pela AWS. |
| **Facilidade de Uso** | **Mais simples.** Possui uma curva de aprendizado menor e integração profunda com o ecossistema AWS. | **Mais complexo.** Requer conhecimento de Kubernetes, mas oferece o poder e a flexibilidade do padrão da indústria. |
| **Ecossistema** | Focado no ecossistema da AWS. | Padrão de código aberto, com uma vasta comunidade e ferramentas de terceiros. Portabilidade entre nuvens. |
| **Componentes** | **Task Definition:** Blueprint para sua aplicação (imagem, CPU, memória, etc.).<br>**Task:** Uma instância em execução de uma Task Definition.<br>**Service:** Mantém um número desejado de Tasks em execução.<br>**Cluster:** Agrupamento lógico de recursos (EC2 ou Fargate). | **Pods:** A menor unidade de implantação (um ou mais contêineres).<br>**Deployments:** Gerencia a criação e o estado dos Pods.<br>**Service:** Expõe os Pods como um serviço de rede.<br>**Cluster:** Um control plane gerenciado pela AWS e worker nodes. |

## 3. Tipos de Lançamento (Launch Types)

Tanto o ECS quanto o EKS precisam de recursos de computação para executar os contêineres. A AWS oferece duas maneiras de fornecer esses recursos:

-   **Tipo de Lançamento EC2:**
    -   **Como funciona:** Você gerencia um cluster de instâncias EC2 (os "worker nodes"). O orquestrador (ECS/EKS) é responsável por agendar e executar os contêineres nessas instâncias.
    -   **Controle:** Você tem controle total sobre o tipo de instância, o sistema operacional, o patching de segurança e a otimização da utilização das instâncias.
    -   **Responsabilidade:** Você é responsável por gerenciar, escalar e proteger a infraestrutura das instâncias EC2.
    -   **Caso de uso:** Cargas de trabalho que exigem controle granular sobre a infraestrutura, acesso a GPUs ou tipos de instância específicos.

-   **Tipo de Lançamento Fargate:**
    -   **Como funciona:** É uma tecnologia **serverless** para contêineres. Você não gerencia nenhum servidor. Basta definir os requisitos de CPU e memória na sua Task Definition (ECS) ou Pod (EKS), e o Fargate provisiona a infraestrutura de computação necessária para executar seus contêineres.
    -   **Controle:** Você não tem acesso às instâncias subjacentes. A AWS gerencia toda a infraestrutura, incluindo patching, escalonamento e segurança do ambiente de execução.
    -   **Responsabilidade:** Sua responsabilidade é focada na aplicação (a imagem do contêiner e sua configuração).
    -   **Caso de uso:** A maioria das cargas de trabalho de contêineres, especialmente microserviços, aplicações web e tarefas em lote, onde você quer focar na aplicação e não na infraestrutura.

### Resumo: EC2 vs. Fargate

| Fator | Tipo de Lançamento EC2 | Tipo de Lançamento Fargate |
| :--- | :--- | :--- |
| **Gerenciamento de Infra** | **Você gerencia** (provisiona, escala, atualiza instâncias EC2). | **AWS gerencia** (Serverless). |
| **Modelo de Preços** | Paga pelas instâncias EC2 (por hora/segundo), independentemente de estarem executando contêineres ou não. | Paga pela quantidade de vCPU e memória que seus contêineres solicitam, pelo tempo que estão em execução. |
| **Controle** | Alto controle sobre o ambiente (tipo de instância, AMIs, etc.). | Menor controle, focado na aplicação. |
| **Simplicidade** | Mais complexo. | Muito mais simples. |

Para o exame SAA-C03, entender a diferença entre ECS e EKS, e especialmente a diferença entre os tipos de lançamento EC2 e Fargate, é crucial. Fargate é frequentemente a resposta para cenários que pedem a solução "mais simples", "com menor sobrecarga operacional" ou "serverless" para contêineres.
