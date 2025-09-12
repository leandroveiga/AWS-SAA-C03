# Monitoramento, Auditoria e Governança na AWS

A capacidade de monitorar, auditar e governar um ambiente na nuvem é fundamental para manter a segurança, a conformidade, o controle de custos e a excelência operacional. A AWS fornece um conjunto robusto de serviços para essas finalidades.

## 1. Amazon CloudWatch

O Amazon CloudWatch é um serviço de monitoramento e observabilidade projetado para engenheiros de DevOps, desenvolvedores, engenheiros de confiabilidade de sites (SREs) e gerentes de TI. Ele fornece dados e insights acionáveis para monitorar suas aplicações, responder a alterações de performance em todo o sistema, otimizar a utilização de recursos e obter uma visão unificada da saúde operacional.

### Principais Componentes do CloudWatch:

-   **Métricas (Metrics):**
    -   São séries temporais de dados. A maioria dos serviços da AWS envia métricas automaticamente para o CloudWatch (ex: utilização de CPU do EC2, número de leituras/escritas no DynamoDB).
    -   **Métricas Padrão vs. Detalhadas:** As métricas padrão para EC2 são coletadas a cada 5 minutos. A "monitoração detalhada" (custo adicional) aumenta a frequência para 1 minuto, essencial para cenários de Auto Scaling rápidos.
    -   **Métricas Customizadas:** Você pode publicar suas próprias métricas usando a API `PutMetricData` (ex: monitorar a performance de uma aplicação, como o tempo de processamento de um pedido).

-   **Alarmes (Alarms):**
    -   Permitem que você defina limites para as métricas e execute ações automaticamente quando esses limites são ultrapassados.
    -   **Ações de Alarme:** Enviar uma notificação para um tópico SNS, acionar uma ação do EC2 Auto Scaling (aumentar/diminuir a frota) ou parar/terminar/reiniciar uma instância EC2.
    -   **Estados de Alarme:** `OK` (dentro do limite), `ALARM` (fora do limite), `INSUFFICIENT_DATA` (não há dados suficientes para determinar o estado).

-   **Logs (Logs):**
    -   **CloudWatch Logs:** Permite centralizar, monitorar e analisar arquivos de log de diversas fontes.
    -   **Agente do CloudWatch Logs:** Pode ser instalado em instâncias EC2 para enviar logs do sistema operacional e de aplicações para o CloudWatch Logs.
    -   **Filtros de Métrica:** Você pode criar métricas a partir de eventos de log (ex: contar o número de erros "404" em logs de acesso de um servidor web) para criar alarmes.

-   **Eventos (Events) / EventBridge:**
    -   O Amazon EventBridge é a evolução do CloudWatch Events e permite construir arquiteturas orientadas a eventos.
    -   Ele reage a eventos que acontecem em sua conta AWS (ex: uma instância EC2 mudou de estado, um arquivo foi carregado no S3) ou a eventos de aplicações SaaS parceiras.
    -   **Regras (Rules):** Uma regra corresponde a eventos recebidos e os roteia para "alvos" (targets) para processamento.
    -   **Alvos (Targets):** Podem ser funções Lambda, tópicos SNS, filas SQS, máquinas de estado do Step Functions, entre outros.

## 2. AWS CloudTrail

O AWS CloudTrail é um serviço que habilita a governança, a conformidade, a auditoria operacional e a auditoria de risco de sua conta AWS. Com o CloudTrail, você pode registrar, monitorar continuamente e reter a atividade da conta relacionada a ações em toda a sua infraestrutura da AWS.

-   **Funcionalidade Principal:** Registra **quem** fez **o quê**, **quando**, **de onde** e **em qual recurso**. Ele audita chamadas de API.
-   **Trilhas (Trails):** Por padrão, o CloudTrail já registra os eventos dos últimos 90 dias. Para reter logs por mais tempo e ter mais controle, você deve criar uma "trilha", que salva os logs em um bucket S3 e, opcionalmente, no CloudWatch Logs.
-   **Casos de Uso:**
    -   **Análise de Segurança:** Identificar acesso não autorizado ou atividades maliciosas. Ex: "Quem deletou aquele bucket S3?".
    -   **Resolução de Problemas Operacionais:** Entender a sequência de eventos que levou a um problema. Ex: "Qual alteração no Security Group causou a perda de conectividade?".
    -   **Rastreamento de Conformidade:** Provar para auditores que as políticas de segurança estão sendo seguidas.

## 3. AWS Config

O AWS Config é um serviço que permite acessar, auditar e avaliar as configurações de seus recursos da AWS. Ele monitora e registra continuamente as configurações dos seus recursos e permite que você automatize a avaliação dessas configurações em relação às configurações ideais desejadas.

-   **CloudTrail vs. Config:**
    -   **CloudTrail** responde "Quem fez a chamada de API para alterar este recurso?".
    -   **AWS Config** responde "Qual é o estado atual deste recurso e como ele mudou ao longo do tempo?". Ele foca na configuração do recurso em si.

-   **Principais Componentes:**
    -   **Itens de Configuração (Configuration Items):** Um registro detalhado da configuração de um recurso em um ponto no tempo.
    -   **Regras do Config (Config Rules):** Permitem verificar a conformidade dos recursos com políticas específicas.
        -   **Regras Gerenciadas:** Regras pré-construídas pela AWS (ex: "verificar se o versionamento está habilitado em todos os buckets S3", "verificar se nenhuma porta SSH (22) está aberta para o mundo").
        -   **Regras Customizadas:** Você pode criar suas próprias regras usando funções Lambda.
    -   **Remediação Automática:** Pode ser integrado com o AWS Systems Manager para executar ações de remediação automaticamente quando um recurso se torna não conforme.

---

## 4. AWS Trusted Advisor

-   **O que é:** Um serviço de orientação em tempo real que ajuda a seguir as melhores práticas da AWS. Ele inspeciona seu ambiente AWS e faz recomendações.
-   **Cinco Pilares:**
    1.  **Otimização de Custos:** Identifica recursos ociosos (ex: instâncias EC2 com baixa utilização, Elastic IPs não associados).
    2.  **Performance:** Verifica a utilização de serviços para garantir alta performance (ex: snapshots de EBS antigos).
    3.  **Segurança:** Recomendações para melhorar a segurança (ex: buckets S3 com acesso público, falta de MFA no usuário root).
    4.  **Tolerância a Falhas:** Sugestões para aumentar a resiliência (ex: implantações RDS não Multi-AZ, falta de backups).
    5.  **Limites de Serviço (Service Quotas):** Verifica o uso em relação aos limites da conta.
-   **Níveis de Acesso:** O plano Basic Support (gratuito) oferece acesso a verificações de segurança e limites de serviço. Planos pagos (Developer, Business, Enterprise) oferecem acesso a todas as verificações.
