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
    -   **Remediação Automática:** O AWS Config pode ser configurado para executar ações de remediação (usando documentos do AWS Systems Manager) quando um recurso é considerado não conforme.

## 4. AWS Organizations e Control Tower

-   **AWS Organizations:**
    -   Permite gerenciar centralmente múltiplas contas AWS.
    -   **Faturamento Consolidado (Consolidated Billing):** Centraliza o pagamento de todas as contas e pode oferecer descontos por volume.
    -   **Unidades Organizacionais (OUs):** Permite agrupar contas para aplicar políticas de gerenciamento (ex: OUs para "Produção", "Desenvolvimento").
    -   **Políticas de Controle de Serviço (SCPs - Service Control Policies):** A funcionalidade mais poderosa. SCPs são "guardrails" que restringem as permissões que podem ser usadas nas contas-membro, mesmo para o usuário `root` da conta. Ex: "Ninguém na OU de Desenvolvimento pode lançar instâncias EC2 do tipo `large` ou superior".

-   **AWS Control Tower:**
    -   É um serviço que automatiza a configuração de um ambiente AWS seguro e com várias contas, chamado de "landing zone".
    -   Ele usa outros serviços por baixo dos panos (como Organizations, IAM, Config) para aplicar as melhores práticas da AWS de forma automatizada.
    -   **Guardrails:** Implementa regras de alto nível para governança (ex: "detectar desvio de configuração", "proibir alterações em regras do CloudTrail").

## 5. Amazon QuickSight

O Amazon QuickSight é um serviço de Business Intelligence (BI) escalável, serverless, e totalmente gerenciado que permite visualizar dados e criar dashboards interativos. Ele se integra nativamente com os serviços da AWS, facilitando a análise de dados armazenados na nuvem.

-   **Principais Casos de Uso:**
    -   **Análise de Negócios:** Criar painéis para acompanhar KPIs (Key Performance Indicators), métricas de vendas, e performance operacional.
    -   **Visualização de Dados:** Transformar grandes volumes de dados brutos (ex: de logs do CloudTrail, dados de custos e uso da AWS, ou dados de aplicações) em gráficos e tabelas fáceis de entender.
    -   **BI Embarcado (Embedded Analytics):** Incorporar dashboards do QuickSight diretamente em suas aplicações, portais e websites, oferecendo análises ricas para seus usuários finais.
-   **Motor de Análise (SPICE):** O QuickSight utiliza o SPICE (Super-fast, Parallel, In-memory Calculation Engine), um motor de cálculo em memória que otimiza as consultas para uma performance rápida, mesmo com grandes conjuntos de dados.

```
