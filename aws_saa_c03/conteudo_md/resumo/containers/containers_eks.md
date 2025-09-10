### O que é Amazon EKS (Elastic Kubernetes Service)

O Amazon Elastic Kubernetes Service (EKS) é um serviço gerenciado que facilita a execução do Kubernetes na AWS sem a necessidade de instalar, operar e manter seu próprio plano de controle (control plane) ou nós do Kubernetes. O Kubernetes é um sistema de orquestração de contêineres de código aberto popular que automatiza a implantação, o escalonamento e o gerenciamento de aplicações em contêineres.

### Como funciona o Amazon EKS

1.  **Plano de Controle Gerenciado (Managed Control Plane):** O EKS provisiona e gerencia o plano de controle do Kubernetes para você em várias Zonas de Disponibilidade da AWS, garantindo alta disponibilidade. O plano de controle é responsável por tarefas como agendar contêineres, gerenciar a disponibilidade da aplicação, armazenar dados do cluster e outras tarefas importantes. O EKS cuida do patching, do escalonamento e dos backups do plano de controle.
2.  **Plano de Dados (Data Plane) / Nós de Trabalho (Worker Nodes):** Você define e executa os nós de trabalho (worker nodes) onde seus contêineres (pods) serão executados. Você tem duas opções principais para provisionar os nós de trabalho:
    *   **Grupos de Nós Gerenciados (Managed Node Groups):** Automatizam o provisionamento e o gerenciamento do ciclo de vida das instâncias EC2 para os nós de trabalho. A AWS lida com patches, atualizações e drenagem de nós.
    *   **AWS Fargate:** Permite executar pods do Kubernetes de forma "serverless", sem a necessidade de provisionar ou gerenciar instâncias EC2. Cada pod é executado em seu próprio ambiente de computação isolado.
3.  **Conexão e Gerenciamento:** Você pode se conectar ao seu cluster EKS usando ferramentas de linha de comando do Kubernetes, como `kubectl`, da mesma forma que faria com qualquer outro cluster Kubernetes.

### Componentes Principais

*   **Plano de Controle do EKS:** O cérebro do cluster Kubernetes, gerenciado pela AWS.
*   **Nós de Trabalho (Worker Nodes):** Instâncias EC2 ou Fargate que executam suas aplicações em contêineres (pods).
*   **Pods:** A menor unidade de implantação no Kubernetes. Um pod representa um ou mais contêineres em execução.
*   **Integração com a AWS:** O EKS se integra profundamente com o ecossistema da AWS, incluindo:
    *   **VPC:** Os pods são executados em uma VPC, permitindo que você use seus próprios grupos de segurança e listas de controle de acesso de rede (NACLs).
    *   **Elastic Load Balancing:** Suporta o uso de Application Load Balancers e Network Load Balancers para expor seus serviços.
    *   **IAM:** Usa o IAM para fornecer autenticação ao seu cluster Kubernetes (via `aws-iam-authenticator`).
    *   **CloudWatch:** Para coletar logs e métricas do plano de controle e das aplicações.

### EKS vs. ECS

| Característica | Amazon EKS | Amazon ECS |
| :--- | :--- | :--- |
| **Orquestrador** | Kubernetes (Padrão da indústria, código aberto). | Orquestrador proprietário da AWS. |
| **Ecossistema** | Vasto ecossistema de ferramentas e plugins da comunidade Kubernetes. | Integração mais profunda e nativa com os serviços da AWS. |
| **Curva de Aprendizado** | Mais íngreme. Requer conhecimento de Kubernetes. | Mais simples e rápido para começar se você já está no ecossistema da AWS. |
| **Flexibilidade** | Altamente flexível e configurável. Portável para outros ambientes Kubernetes. | Mais opinativo, mas mais simples de operar. |

### Benefícios do EKS

*   **Kubernetes Gerenciado:** Remove a carga operacional de gerenciar o plano de controle do Kubernetes.
*   **Disponibilidade e Segurança:** Executa um plano de controle altamente disponível e seguro em várias AZs.
*   **Comunidade e Portabilidade:** Permite que você aproveite todas as ferramentas e plugins da comunidade Kubernetes e evite o aprisionamento tecnológico (vendor lock-in).
*   **Flexibilidade do Plano de Dados:** Oferece a escolha entre o controle das instâncias EC2 e a simplicidade do Fargate.
