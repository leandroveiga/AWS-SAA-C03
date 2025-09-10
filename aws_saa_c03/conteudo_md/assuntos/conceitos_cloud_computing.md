# Conceitos Fundamentais de Cloud Computing

**Cloud Computing**, ou Computação em Nuvem, é a entrega de recursos de computação—como servidores, armazenamento, bancos de dados e software—pela internet com um modelo de precificação de pagamento conforme o uso. Em vez de comprar e manter seus próprios data centers, você pode acessar serviços de tecnologia de um provedor de nuvem como a AWS.

### Características Essenciais da Nuvem:

-   **Autosserviço sob demanda:** Provisione recursos sem intervenção humana.
-   **Amplo acesso à rede:** Acesse recursos pela internet de qualquer lugar.
-   **Pool de recursos:** O provedor agrupa recursos para servir múltiplos clientes.
-   **Rápida elasticidade:** Escale recursos para cima ou para baixo de forma rápida e automática.
-   **Serviço mensurado:** Pague apenas pelo que você usa.

## Modelos de Serviço em Nuvem
<img src="../../img/servicos_cloud2.png"/>

| Modelo | O que você gerencia | O que o provedor gerencia | Exemplo AWS |
| :--- | :--- | :--- | :--- |
| **IaaS** | Aplicações, Dados, Runtime, Middleware, SO | Virtualização, Servidores, Armazenamento, Rede | **Amazon EC2** |
| **PaaS** | Aplicações, Dados | Runtime, Middleware, SO, Virtualização, etc. | **AWS Elastic Beanstalk** |
| **SaaS** | Nada (apenas usa o software) | Tudo | **Amazon WorkMail** |

### IaaS (Infrastructure as a Service - Infraestrutura como Serviço)
-   **O que é**: Fornece os blocos de construção fundamentais da infraestrutura de TI: máquinas virtuais, armazenamento e redes.
-   **Nível de Controle**: Máximo controle sobre o hardware virtualizado.
-   **Exemplo Principal**: **Amazon EC2** (Elastic Compute Cloud), onde você aluga servidores virtuais.

### PaaS (Platform as a Service - Plataforma como Serviço)
-   **O que é**: Remove a necessidade de gerenciar a infraestrutura subjacente (hardware e sistemas operacionais) e permite que você se concentre na implantação e gerenciamento de suas aplicações.
-   **Foco**: No desenvolvimento de aplicações, não na infraestrutura.
-   **Exemplo Principal**: **AWS Elastic Beanstalk**, onde você faz o upload do seu código e a plataforma cuida do resto.

### SaaS (Software as a Service - Software como Serviço)
-   **O que é**: Fornece um produto completo, executado e gerenciado pelo provedor de serviços. O usuário final simplesmente utiliza o software.
-   **Foco**: Utilização do software, sem se preocupar com nada da infraestrutura.
-   **Exemplo**: O próprio Gmail ou, no mundo AWS, o **Amazon WorkMail**.

### Outros Modelos Relevantes:

-   **CaaS (Containers as a Service):** Plataforma para gerenciar contêineres. **Exemplos AWS:** Amazon ECS (Elastic Container Service) e EKS (Elastic Kubernetes Service).
-   **FaaS (Functions as a Service / Serverless):** Execute código em resposta a eventos sem gerenciar servidores. **Exemplo AWS:** **AWS Lambda**.

<img src="../../img/servicos_cloud.png"/>

## Tipos de Nuvem

<img src="../../img/tipos_cloud.png"/>

### Nuvem Pública:
-   **O que é**: A infraestrutura de nuvem é de propriedade e operada por um provedor de nuvem terceirizado (como AWS, Google Cloud, Azure) e os recursos são compartilhados por várias organizações pela internet.
-   **Vantagens**: Custo-benefício, sem manutenção de hardware, escalabilidade quase ilimitada.

### Nuvem Privada:
-   **O que é**: Os recursos de computação são usados exclusivamente por uma única empresa ou organização. Pode ser localizada no data center local da empresa ou hospedada por um provedor de serviços terceirizado.
-   **Vantagens**: Maior controle, segurança e privacidade.

### Nuvem Híbrida:
-   **O que é**: Combina nuvens privadas e públicas, unidas por tecnologia que permite que dados e aplicações sejam compartilhados entre elas.
-   **Vantagens**: Flexibilidade para manter aplicações críticas na nuvem privada enquanto aproveita a escalabilidade da nuvem pública para cargas de trabalho menos sensíveis.

## Modelo de Responsabilidade Compartilhada na Nuvem
<img src="../../img/responsabilidade_compartilhada.png">

Este é um conceito **crítico** para o exame. Ele define quem é responsável pelo quê.

-   **AWS (Responsabilidade *DA* Nuvem):**
    -   A AWS é responsável por proteger a infraestrutura global que executa todos os serviços. Isso inclui o hardware, software, rede e as instalações físicas (Regiões, Zonas de Disponibilidade, Pontos de Presença).
    -   **Analogia:** A AWS constrói e protege o prédio de apartamentos.

-   **Cliente (Responsabilidade *NA* Nuvem):**
    -   Sua responsabilidade é determinada pelos serviços que você escolhe. Você é responsável por gerenciar e proteger seus dados, configurar o acesso (IAM), gerenciar o sistema operacional (no caso do IaaS como EC2), configurar firewalls (Security Groups) e criptografar seus dados.
    -   **Analogia:** Você é responsável por trancar a porta do seu apartamento e por tudo o que acontece dentro dele.

-   **Regra geral:**
    -   **Você** é responsável por tudo que você **configura** e **coloca** na nuvem.
    -   **AWS** é responsável pela infraestrutura que **suporta** a nuvem.

