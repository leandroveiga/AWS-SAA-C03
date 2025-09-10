### O que é AWS Fargate

O AWS Fargate é um mecanismo de computação sem servidor (serverless) para contêineres que funciona tanto com o Amazon Elastic Container Service (ECS) quanto com o Amazon Elastic Kubernetes Service (EKS). O Fargate elimina a necessidade de provisionar e gerenciar servidores, permitindo que você se concentre na criação de aplicações. Com o Fargate, você não precisa mais se preocupar em escolher tipos de instância, gerenciar o escalonamento de clusters ou aplicar patches nos sistemas operacionais dos hosts.

### Como funciona o AWS Fargate

1.  **Empacotamento da Aplicação:** Você empacota sua aplicação em um ou mais contêineres (por exemplo, usando Docker).
2.  **Definição de Recursos:** Você define os recursos que sua aplicação precisa por meio de uma **Definição de Tarefa (Task Definition)** no ECS ou de uma **Especificação de Pod (Pod Spec)** no EKS. Nesta definição, você especifica:
    *   A imagem do contêiner a ser usada.
    *   A quantidade de **CPU** e **Memória** que a tarefa/pod necessita.
    *   Políticas de rede e permissões do IAM.
3.  **Lançamento:** Você lança seus contêineres usando o Fargate como o "tipo de lançamento" (launch type).
4.  **Provisionamento e Gerenciamento pela AWS:** O Fargate lê suas especificações e lança a quantidade exata de recursos de computação necessários para executar seus contêineres, em um ambiente totalmente gerenciado e isolado. Ele lida com todo o provisionamento, escalonamento e manutenção da infraestrutura subjacente.
5.  **Faturamento:** Você paga apenas pela quantidade de vCPU e recursos de memória que seus contêineres solicitam, e apenas pelo tempo que eles estão em execução. O faturamento é por segundo, com um mínimo de um minuto.

### Fargate com ECS vs. Fargate com EKS

*   **Fargate com ECS:** É a maneira mais simples de executar contêineres na AWS. A integração é profunda e a experiência é muito simplificada. Você usa os conceitos do ECS (Definições de Tarefa, Serviços, Clusters) e simplesmente especifica "Fargate" como o tipo de lançamento.
*   **Fargate com EKS:** Permite que você execute seus pods do Kubernetes sem gerenciar instâncias EC2. Você cria um "Perfil do Fargate" que especifica quais pods devem ser executados no Fargate. Quando os pods são lançados em namespaces que correspondem ao perfil, o EKS os agenda no Fargate. Isso permite uma arquitetura mista, onde você pode ter alguns nós de trabalho EC2 para cargas de trabalho específicas e usar o Fargate para outras.

### Benefícios do Fargate

*   **Computação sem Servidor (Serverless):** Elimina a necessidade de gerenciar a infraestrutura subjacente. Não há clusters para gerenciar, nem instâncias para aplicar patches.
*   **Segurança Aprimorada por Design:** Cada tarefa ou pod executado no Fargate tem seu próprio limite de isolamento e não compartilha o kernel subjacente, CPU, memória ou interface de rede elástica com outras tarefas ou pods.
*   **Foco no Desenvolvimento:** Permite que os desenvolvedores se concentrem em projetar e construir suas aplicações em vez de gerenciar a infraestrutura.
*   **Escalonamento Transparente:** Escala os recursos de computação necessários de forma transparente para atender aos requisitos de suas aplicações.
*   **Custo-Benefício:** O modelo de pagamento por uso garante que você pague apenas pelos recursos que seus contêineres consomem enquanto estão em execução.

### Quando usar Fargate vs. EC2 Launch Type

*   **Use Fargate quando:** Você quer a maior simplicidade operacional, não quer gerenciar instâncias EC2, tem cargas de trabalho que se encaixam bem no modelo de CPU/memória do Fargate e quer se beneficiar do isolamento de segurança por tarefa/pod.
*   **Use EC2 Launch Type quando:** Você precisa de mais controle sobre suas instâncias (por exemplo, tipos de instância específicos com GPUs, otimização de rede), precisa de acesso ao sistema de arquivos do host, ou tem uma estratégia de otimização de custos que envolve Instâncias Spot ou Savings Plans de uma maneira que o Fargate não suporta.
