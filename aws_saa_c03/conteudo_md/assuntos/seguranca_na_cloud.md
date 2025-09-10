# Segurança na Nuvem AWS

A segurança na AWS é uma responsabilidade compartilhada e uma prioridade máxima. A AWS oferece um amplo conjunto de serviços para proteger dados, infraestrutura e aplicações contra ameaças.

---

## 1. Modelo de Responsabilidade Compartilhada

Este é o conceito mais fundamental da segurança na AWS.

-   **AWS é responsável pela segurança *DA* nuvem:**
    -   Proteção da infraestrutura física (data centers, hardware, rede).
    -   Segurança do software que executa os serviços da AWS (hipervisor, etc.).

-   **Você (o cliente) é responsável pela segurança *NA* nuvem:**
    -   **Gerenciamento de identidade e acesso (IAM):** Configurar usuários, grupos, roles e políticas corretamente.
    -   **Proteção de dados:** Criptografar dados em trânsito e em repouso.
    -   **Configuração de rede:** Configurar VPCs, sub-redes, Security Groups e NACLs.
    -   **Configuração do lado do cliente:** Gerenciar o sistema operacional, patches de segurança e o firewall em instâncias EC2.

## 2. AWS WAF (Web Application Firewall)

O AWS WAF é um firewall de aplicação web que ajuda a proteger suas aplicações web ou APIs contra exploits da web comuns que podem afetar a disponibilidade, comprometer a segurança ou consumir recursos excessivos.

-   **Como funciona:** Ele inspeciona o tráfego HTTP/HTTPS que chega às suas aplicações e permite que você crie regras para bloquear padrões de ataque, como:
    -   **Injeção de SQL (SQL Injection)**
    -   **Cross-Site Scripting (XSS)**
-   **Onde ele atua:** O WAF pode ser implantado em:
    -   **Amazon CloudFront** (para proteger o conteúdo na borda)
    -   **Application Load Balancer (ALB)**
    -   **Amazon API Gateway**
-   **Regras:** Você pode usar regras gerenciadas pela AWS ou por parceiros (Managed Rule Sets) ou criar suas próprias regras customizadas.

## 3. AWS Shield

O AWS Shield é um serviço gerenciado de proteção contra ataques de Negação de Serviço Distribuída (DDoS).

-   **AWS Shield Standard:**
    -   **Custo:** Gratuito.
    -   **Proteção:** Ativado automaticamente para todos os clientes da AWS. Oferece proteção contra os ataques DDoS mais comuns que visam a infraestrutura (camadas 3 e 4 da rede).

-   **AWS Shield Advanced:**
    -   **Custo:** Pago (taxa mensal + taxas de transferência de dados).
    -   **Proteção:** Oferece um nível muito mais alto de proteção para aplicações em execução no EC2, ELB, CloudFront, Global Accelerator e Route 53.
    -   **Recursos Adicionais:**
        -   Detecção e mitigação de ataques sofisticados na camada de aplicação (camada 7).
        -   Visibilidade quase em tempo real dos ataques.
        -   Acesso 24/7 à Equipe de Resposta a DDoS da AWS (DRT).
        -   **Proteção de custos:** Protege contra picos de cobrança em seus serviços AWS que podem resultar de um ataque DDoS.

## 4. AWS Key Management Service (KMS)

O AWS KMS é um serviço gerenciado que facilita a criação e o controle das chaves de criptografia usadas para criptografar seus dados.

-   **Conceito Principal:** O KMS não armazena seus dados, ele armazena e gerencia as chaves que criptografam seus dados.
-   **Tipos de Chaves (CMK - Customer Master Key):**
    -   **Gerenciada pela AWS (AWS Managed CMK):** Criadas e gerenciadas pela AWS para uso em um serviço específico (ex: a chave padrão para criptografia do S3).
    -   **Gerenciada pelo Cliente (Customer Managed CMK):** Você cria, gerencia e controla a política de acesso da chave. Você tem mais controle, mas também mais responsabilidade. É a única que pode ser usada para criptografia do lado do cliente.
    -   **Importada:** Você pode importar seu próprio material de chave de seu ambiente on-premises.
-   **Criptografia de Envelope (Envelope Encryption):**
    -   Este é o processo que o KMS usa. Em vez de enviar grandes volumes de dados para o KMS criptografar, o processo é:
        1.  O KMS gera uma chave de dados única (Data Key) a partir da CMK.
        2.  O KMS retorna duas versões da chave de dados para o serviço: uma em texto plano e outra criptografada pela CMK.
        3.  O serviço usa a chave de dados em texto plano para criptografar os dados localmente.
        4.  A chave de dados em texto plano é descartada da memória.
        5.  A chave de dados criptografada é armazenada junto com os dados criptografados.
    -   Para descriptografar, o processo é o inverso: o serviço envia a chave de dados criptografada para o KMS, que a descriptografa usando a CMK e retorna a chave de dados em texto plano para o serviço descriptografar os dados.

<p align="center">
    <img alt="Hierarquia de chaves KMS" width="560" src="https://docs.aws.amazon.com/pt_br/kms/latest/developerguide/images/CMK-Hierarchy.png" />
    <br/><em>Fonte: AWS KMS Developer Guide (Hierarquia de chaves)</em>
</p>

## 5. AWS Inspector

O Amazon Inspector é um serviço de gerenciamento de vulnerabilidades que verifica continuamente suas cargas de trabalho da AWS (instâncias EC2 e imagens de contêiner no ECR) em busca de vulnerabilidades de software e exposição não intencional à rede.

-   **Como funciona:** Ele descobre automaticamente as cargas de trabalho e as verifica em busca de vulnerabilidades conhecidas (CVEs) e problemas de alcance da rede.
-   **Painel de Controle:** Fornece uma pontuação de risco para cada descoberta, ajudando a priorizar os esforços de remediação.
-   **Inspector vs. WAF:**
    -   **Inspector:** É proativo. Ele olha para *dentro* da sua instância EC2 ou imagem de contêiner para encontrar vulnerabilidades no software *antes* que sejam exploradas.
    -   **WAF:** É reativo. Ele fica na *frente* da sua aplicação para bloquear ataques que tentam explorar vulnerabilidades.

---
