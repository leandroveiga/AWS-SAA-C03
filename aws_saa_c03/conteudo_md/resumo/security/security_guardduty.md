### O que é Amazon GuardDuty

O Amazon GuardDuty é um serviço de detecção de ameaças inteligente que monitora continuamente suas contas e cargas de trabalho da AWS em busca de atividades maliciosas ou comportamento não autorizado. Ele usa machine learning, detecção de anomalias e inteligência de ameaças integrada para identificar e priorizar ameaças potenciais. O GuardDuty analisa dezenas de bilhões de eventos em várias fontes de dados da AWS, como logs do AWS CloudTrail, logs de fluxo da VPC e logs de DNS.

### Como funciona o Amazon GuardDuty

1.  **Habilitação Simples:** Você pode habilitar o GuardDuty com um único clique no Console de Gerenciamento da AWS. Não há software para implantar ou infraestrutura de segurança para gerenciar.
2.  **Análise de Fontes de Dados:** Uma vez habilitado, o GuardDuty começa imediatamente a analisar fluxos de dados contínuos em tempo real. As principais fontes de dados são:
    *   **Logs de Eventos do AWS CloudTrail:** Analisa chamadas de API para detectar atividades incomuns, como implantações em uma região não utilizada, desativação de logs ou chamadas de API de locais maliciosos conhecidos.
    *   **Logs de Fluxo da VPC (VPC Flow Logs):** Analisa o tráfego de rede de e para suas instâncias EC2 para identificar padrões anômalos, como varreduras de portas, instâncias se comunicando com servidores de Comando e Controle (C2) ou transferências de dados para endereços IP maliciosos.
    *   **Logs de DNS:** Monitora as consultas de DNS feitas por seus recursos da AWS para identificar a comunicação com domínios associados a malware ou phishing.
3.  **Inteligência de Ameaças e Machine Learning:**
    *   **Inteligência de Ameaças:** O GuardDuty é integrado com inteligência de ameaças da AWS e de provedores terceirizados, como a Proofpoint e a CrowdStrike. Ele usa listas de endereços IP e domínios maliciosos conhecidos para detectar atividades suspeitas.
    *   **Machine Learning (ML):** O GuardDuty estabelece uma linha de base (baseline) do comportamento normal e esperado em sua conta. Em seguida, ele usa modelos de ML para detectar anomalias e desvios dessa linha de base, como uma instância EC2 que de repente começa a se comunicar em uma porta ou protocolo incomum.
4.  **Geração de Descobertas (Findings):** Quando o GuardDuty detecta uma ameaça potencial, ele gera uma "descoberta" de segurança detalhada. Cada descoberta inclui informações sobre o recurso afetado, a natureza da ameaça e um nível de severidade (Baixo, Médio, Alto).
5.  **Remediação e Resposta:** As descobertas do GuardDuty são enviadas para o Amazon EventBridge (anteriormente CloudWatch Events). Você pode usar o EventBridge para acionar ações de remediação automatizadas, como:
    *   Invocar uma função do **AWS Lambda** para isolar uma instância EC2 comprometida (por exemplo, alterando seu grupo de segurança).
    *   Enviar uma notificação para uma equipe de segurança via **Amazon SNS**.
    *   Integrar-se com sistemas de SIEM (Security Information and Event Management) de terceiros.

### Exemplos de Descobertas do GuardDuty

*   **Reconnaissance (Reconhecimento):** Atividade que sugere que um ator malicioso está sondando sua rede (ex: varredura de portas por um IP malicioso conhecido).
*   **UnauthorizedAccess (Acesso Não Autorizado):** Tentativas de força bruta de senhas SSH ou RDP, ou chamadas de API de uma fonte suspeita.
*   **Trojan:** Uma instância EC2 se comportando como um bot de negação de serviço (DoS) ou minerando criptomoedas.
*   **CommandAndControl (Comando e Controle):** Uma instância EC2 se comunicando com um servidor C2 conhecido.
*   **Exfiltration (Exfiltração):** Grandes quantidades de dados sendo enviadas para fora de uma instância EC2, o que pode indicar roubo de dados.

### Benefícios do GuardDuty

*   **Detecção Inteligente de Ameaças:** Vai além da simples correspondência de assinaturas, usando ML e detecção de anomalias para encontrar ameaças desconhecidas.
*   **Fácil de Habilitar e Gerenciar:** Um serviço de "um clique" que não requer implantação ou manutenção de agentes ou appliances.
*   **Ampla Cobertura:** Analisa várias fontes de dados da AWS para fornecer uma visão abrangente da segurança.
*   **Custo-Benefício:** Você paga apenas pelos eventos analisados, sem taxas iniciais.
*   **Automação da Resposta:** A integração com o EventBridge permite a criação de fluxos de trabalho de resposta a incidentes automatizados.
