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

## 5. AWS Certificate Manager (ACM)

O ACM é um serviço que permite provisionar, gerenciar e implantar certificados SSL/TLS públicos e privados para uso com serviços da AWS e seus recursos internos.

-   **Certificados Públicos Gratuitos:** O ACM fornece certificados SSL/TLS públicos gratuitos que você pode usar em serviços integrados como **Elastic Load Balancing (ALB/NLB)** e **Amazon CloudFront**.
-   **Renovação Automática:** O ACM gerencia a renovação automática dos certificados, eliminando a necessidade de processos manuais.
-   **Segurança:** As chaves privadas dos certificados são protegidas e gerenciadas pela AWS.

## 6. AWS Inspector

O Amazon Inspector é um serviço de gerenciamento de vulnerabilidades que verifica continuamente suas cargas de trabalho da AWS (instâncias EC2 e imagens de contêiner no ECR) em busca de vulnerabilidades de software e exposição não intencional à rede.

-   **Como funciona:** Ele descobre automaticamente as cargas de trabalho e as verifica em busca de vulnerabilidades conhecidas (CVEs) e problemas de alcance da rede.
-   **Painel de Controle:** Fornece uma pontuação de risco para cada descoberta, ajudando a priorizar os esforços de remediação.
-   **Inspector vs. WAF:**
    -   **Inspector:** É proativo. Ele olha para *dentro* da sua instância EC2 ou imagem de contêiner para encontrar vulnerabilidades no software *antes* que sejam exploradas.
    -   **WAF:** É reativo. Ele fica na *frente* da sua aplicação para bloquear ataques que tentam explorar vulnerabilidades.

## 7. Amazon GuardDuty

O Amazon GuardDuty é um serviço de detecção de ameaças que monitora continuamente sua conta e cargas de trabalho da AWS em busca de atividades maliciosas ou comportamento não autorizado.

-   **Como funciona:** Ele analisa e processa dados de várias fontes, incluindo:
    -   **Logs do AWS CloudTrail:** Para detectar chamadas de API incomuns ou maliciosas.
    -   **Logs de Fluxo da VPC (VPC Flow Logs):** Para identificar tráfego de rede suspeito.
    -   **Logs de DNS:** Para encontrar comunicação com domínios maliciosos conhecidos.
-   **Inteligência de Ameaças:** O GuardDuty usa machine learning, detecção de anomalias e inteligência de ameaças integrada (listas de IPs e domínios maliciosos) para identificar ameaças com precisão.
-   **Tipos de Descobertas (Findings):** Ele gera descobertas de segurança detalhadas, como:
    -   Uma instância EC2 se comunicando com um servidor de Comando e Controle (C&C) conhecido.
    -   Tentativas de força bruta contra uma porta RDP/SSH.
    -   Atividade de API incomum, como o lançamento de instâncias em uma região não utilizada.
-   **GuardDuty vs. Inspector:**
    -   **Inspector:** Procura por vulnerabilidades *conhecidas* (CVEs) em seu software.
    -   **GuardDuty:** Procura por atividades *maliciosas ativas* em sua conta. Ele não sabe se seu software está vulnerável, mas sabe se algo ou alguém está tentando explorá-lo.

## 8. AWS Network Firewall

O AWS Network Firewall é um serviço de firewall de rede, gerenciado e de alta disponibilidade, para sua Virtual Private Cloud (VPC). Ele facilita a implantação de proteções de rede essenciais para todas as suas VPCs.

-   **Como funciona:** O serviço é implantado em sua VPC e pode ser configurado para filtrar o tráfego que entra e sai da VPC. Você pode criar regras de firewall stateful e stateless, além de usar regras de prevenção de intrusão (IPS) para bloquear ameaças.
-   **Principais Casos de Uso:**
    -   **Inspeção de Tráfego:** Inspecionar e filtrar o tráfego que vai para a internet a partir da sua VPC, entre VPCs ou vindo de ambientes on-premises (via Direct Connect ou VPN).
    -   **Prevenção de Ameaças:** Bloquear a saída de tráfego para domínios maliciosos conhecidos e impedir que vulnerabilidades sejam exploradas.
    -   **Filtragem Web:** Filtrar o tráfego da web de saída com base em nomes de domínio totalmente qualificados (FQDN).
-   **Network Firewall vs. WAF vs. Security Group/NACL:**
    -   **Security Group/NACL:** São controles de segurança fundamentais. Security Groups são stateful e agem no nível da instância, enquanto NACLs são stateless e agem no nível da sub-rede.
    -   **Network Firewall:** É uma camada de proteção mais avançada que os Security Groups/NACLs. Ele oferece recursos como inspeção profunda de pacotes (DPI), prevenção de intrusão e filtragem de URL, aplicando-se a todo o tráfego da VPC que você roteia através dele.
    -   **WAF:** Foca na proteção da camada de aplicação (Camada 7), protegendo aplicações web contra ataques como SQL injection e XSS. O Network Firewall protege em um nível mais baixo (Camadas 3-4) e também oferece algumas proteções de camada 7.

## 9. AWS Firewall Manager

O AWS Firewall Manager é um serviço de gerenciamento de segurança que simplifica a administração e manutenção de regras de firewall em múltiplas contas e recursos da sua AWS Organization.

-   **Como funciona:** Ele permite que você configure centralmente as regras de firewall e as políticas de segurança e as aplique de forma consistente em toda a sua organização. Se um novo recurso ou conta for adicionado, o Firewall Manager aplica automaticamente as políticas a ele, garantindo a conformidade.
-   **Serviços Gerenciados:** O Firewall Manager pode gerenciar centralmente:
    -   **Regras do AWS WAF:** Para proteger suas aplicações web.
    -   **Políticas do AWS Shield Advanced:** Para proteção contra DDoS.
    -   **Grupos de regras do AWS Network Firewall:** Para proteger suas VPCs.
    -   **Grupos de Segurança (Security Groups):** Para auditar e controlar grupos de segurança em suas VPCs.
-   **Principal Benefício:** Garante que as políticas de segurança sejam aplicadas de forma uniforme e ajuda a manter a conformidade, especialmente em ambientes grandes e com várias contas.

## 10. Amazon Macie

O Amazon Macie é um serviço de segurança e privacidade de dados totalmente gerenciado que usa machine learning e correspondência de padrões para descobrir e proteger dados sensíveis no Amazon S3.

-   **Como funciona:** O Macie processa os dados nos seus buckets do S3 para identificar e classificar informações confidenciais. Ele fornece um inventário dos seus buckets, alertando sobre aqueles que estão publicamente acessíveis, não criptografados ou compartilhados com contas externas.
-   **Descoberta de Dados Sensíveis:** O Macie pode identificar uma ampla gama de dados sensíveis, como:
    -   **Informações de Identificação Pessoal (PII):** Nomes, endereços, números de CPF, etc.
    -   **Credenciais:** Chaves de acesso da AWS, chaves privadas.
    -   **Dados Financeiros:** Números de cartão de crédito, informações de contas bancárias.
-   **Principais Casos de Uso:**
    -   **Conformidade (Compliance):** Ajudar a atender a regulamentações de privacidade como GDPR, HIPAA e LGPD, identificando onde os dados regulamentados estão armazenados.
    -   **Prevenção de Vazamento de Dados:** Alertar sobre configurações de segurança inadequadas em buckets S3 e a presença de dados sensíveis em locais de risco.
    -   **Visibilidade da Postura de Segurança de Dados:** Fornecer um painel para entender onde seus dados sensíveis residem e como estão protegidos.

---
