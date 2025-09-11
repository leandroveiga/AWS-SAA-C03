# Amazon EC2 (Elastic Compute Cloud)

O **Amazon EC2** é um serviço web que fornece capacidade computacional segura e redimensionável na nuvem. Ele foi projetado para facilitar a computação em escala de web para os desenvolvedores. Essencialmente, o EC2 permite que você alugue servidores virtuais, conhecidos como **instâncias**, para executar suas aplicações.

---

Nota: Uso de imagem diretamente hospedada em docs.aws.amazon.com conforme solicitado.

Referência da imagem e conteúdo: Documentação oficial Amazon EC2 – https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/concepts.html

---

## Componentes Fundamentais de uma Instância EC2

-   **Amazon Machine Image (AMI):** É o modelo para sua instância. Uma AMI inclui um sistema operacional, um servidor de aplicação e aplicações. Você pode escolher AMIs fornecidas pela AWS, pela comunidade ou criar as suas próprias.
-   **Tipos de Instância:** O EC2 oferece uma vasta variedade de tipos de instância otimizados para diferentes casos de uso (ex: computação geral, otimizada para computação, memória, armazenamento ou acelerada).
    
    <img alt="Tipos de instância EC2 - famílias" width="620" src="https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/images/instance-types.png" />
-   **Armazenamento:**
    -   **Elastic Block Store (EBS):** Volumes de armazenamento em nível de bloco, persistentes e de alta performance, que podem ser anexados a uma instância. Pense neles como os "discos rígidos" da sua instância.
    -   **Instance Store:** Armazenamento temporário em nível de bloco localizado nos discos do servidor físico que hospeda a instância. Os dados em um instance store **são perdidos** quando a instância é parada, hibernada ou terminada.
-   **Rede e Segurança:**
    -   **Virtual Private Cloud (VPC):** Uma rede virtual isolada onde suas instâncias são executadas.
    -   **Security Groups:** Atuam como um firewall virtual para suas instâncias, controlando o tráfego de entrada e saída. São *stateful*.
    -   **Elastic IP (EIP):** Um endereço IPv4 público e estático que você pode alocar para sua conta e associar a uma instância para que ela tenha um IP fixo.
    -   **Key Pair (Par de Chaves):** Credenciais de segurança que você usa para provar sua identidade ao se conectar a uma instância (usando SSH para Linux ou RDP para Windows).

### Metadados da Instância e User Data

-   **User Data:** É um script que você pode fornecer ao lançar uma instância EC2. Esse script é executado **apenas uma vez**, na primeira inicialização da instância. É comumente usado para realizar tarefas de configuração automatizadas, como instalar pacotes, aplicar patches ou baixar código.

-   **Instance Metadata Service (IMDS):** É um serviço disponível em um endereço IP especial (`169.254.169.254`) que pode ser acessado *de dentro* da instância EC2. Ele fornece metadados sobre a própria instância, como seu ID, tipo, Zona de Disponibilidade, e credenciais de segurança temporárias associadas a uma IAM Role. É a maneira segura pela qual as aplicações em uma instância EC2 obtêm permissões para interagir com outros serviços da AWS.

---

## Modelos de Compra e Preços do EC2

Este é um tópico **extremamente importante** para o exame.

| Modelo | Ideal para | Como funciona |
| :--- | :--- | :--- |
| **On-Demand** | Cargas de trabalho com picos, imprevisíveis, ou para desenvolvimento/teste. | Pague por segundo (ou hora), sem compromisso de longo prazo. Mais flexível, porém mais caro. |
| **Savings Plans** | Cargas de trabalho com uso consistente e previsível. | Comprometa-se com uma quantidade consistente de uso de computação (ex: $10/hora) por 1 ou 3 anos para obter um grande desconto. **É o modelo mais flexível e recomendado para economia.** |
| **Reserved Instances** | Cargas de trabalho com uso muito estável e previsível (ex: um banco de dados). | Comprometa-se com uma configuração de instância específica (família, região) por 1 ou 3 anos para obter o maior desconto. Menos flexível que os Savings Plans. |
| **Spot Instances** | Cargas de trabalho tolerantes a falhas, sem estado, ou com tempo flexível (ex: processamento em lote, renderização). | Use a capacidade computacional não utilizada da AWS com até 90% de desconto. A AWS pode interromper suas instâncias com um aviso de 2 minutos. |
| **Dedicated Hosts** | Cargas de trabalho com requisitos de conformidade ou licenciamento de software específicos (BYOL - Bring Your Own License). | Pague por um servidor físico inteiro dedicado ao seu uso. Máximo controle e isolamento. |
| **Dedicated Instances**| Instâncias que rodam em hardware dedicado a uma única conta. | Não fornece a visibilidade e o controle de um Dedicated Host, sendo uma opção menos comum hoje em dia. |

### Tipos de Savings Plans
-   **Compute Savings Plans:** Mais flexível. Aplica-se a EC2, Fargate e Lambda, independentemente da família da instância, tamanho, SO ou região.
-   **EC2 Instance Savings Plans:** Maior desconto (até 72%). Compromisso com uma família de instâncias específica em uma região específica.

---

## Ciclo de Vida de uma Instância EC2

-   **Pending:** A instância está sendo preparada para iniciar.
-   **Running:** A instância está em execução e operacional. A cobrança começa.
-   **Stopping/Stopped:** A instância está sendo desligada. Em estado *Stopped*, você não é cobrado pelo uso da instância, mas **é cobrado pelo armazenamento do volume EBS anexado**. Os dados no volume EBS são preservados.
-   **Terminating/Terminated:** A instância está sendo permanentemente excluída. Todos os volumes EBS associados com a configuração "Delete on Termination" são excluídos. Os dados no *Instance Store* são sempre perdidos.

<p align="center">
    <img alt="Ciclo de vida da instância EC2" width="560" src="https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/images/instance_lifecycle.png" />
    <br/><em>Fonte: AWS EC2 User Guide (Instance Lifecycle)</em>
</p>

---

## Placement Groups (Grupos de Posicionamento)

Placement Groups são uma forma de influenciar a localização física das suas instâncias EC2 para otimizar a performance.

-   **Cluster:** Agrupa as instâncias em um único rack na mesma Zona de Disponibilidade.
    -   **Benefício:** Latência extremamente baixa e alta largura de banda entre as instâncias.
    -   **Risco:** Uma falha no rack afeta todas as instâncias.
    -   **Caso de Uso:** Aplicações de computação de alta performance (HPC).

-   **Spread:** Distribui cada instância em um hardware (rack) distinto.
    -   **Benefício:** Maximiza a disponibilidade e reduz o risco de falhas simultâneas.
    -   **Risco:** Pode haver uma latência ligeiramente maior entre as instâncias.
    -   **Caso de Uso:** Aplicações críticas que precisam de alto grau de isolamento (ex: um pequeno número de servidores de banco de dados).

-   **Partition:** Distribui as instâncias em partições lógicas, e cada partição tem seu próprio conjunto de racks.
    -   **Benefício:** Equilibra a necessidade de isolamento com a visibilidade da topologia. Permite que grandes sistemas distribuídos (como HDFS, HBase, Cassandra) reduzam a probabilidade de falhas correlacionadas.
    -   **Caso de Uso:** Aplicações de Big Data que precisam ser cientes da topologia do hardware.

