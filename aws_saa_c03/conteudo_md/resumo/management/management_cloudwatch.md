### O que é Amazon CloudWatch

O Amazon CloudWatch é um serviço de monitoramento e observabilidade para recursos da AWS e as aplicações que você executa na AWS. Você pode usar o CloudWatch para coletar e rastrear métricas, coletar e monitorar arquivos de log, definir alarmes e reagir automaticamente a alterações em seus recursos da AWS.

### Como funciona o Amazon CloudWatch

O CloudWatch pode ser dividido em três componentes principais:

#### 1. CloudWatch Metrics (Métricas)
*   **O que são:** Uma série temporal de pontos de dados. Pense em uma métrica como uma variável a ser monitorada (por exemplo, a utilização da CPU de uma instância EC2).
*   **Como funciona:** Muitos serviços da AWS enviam métricas automaticamente para o CloudWatch (ex: EC2, S3, RDS). Você também pode publicar suas próprias métricas personalizadas (custom metrics) a partir de suas aplicações.
*   **Namespaces:** As métricas são agrupadas por "namespaces". Por exemplo, todas as métricas do EC2 estão no namespace `AWS/EC2`.
*   **Dimensões:** Uma métrica pode ser identificada exclusivamente por seu nome e por uma ou mais dimensões (pares de nome/valor), como `InstanceId=i-12345`.
*   **Visualização:** Você pode criar gráficos e painéis (dashboards) para visualizar suas métricas ao longo do tempo.

#### 2. CloudWatch Alarms (Alarmes)
*   **O que são:** Permitem que você observe uma única métrica do CloudWatch durante um período que você especifica e execute uma ou mais ações com base no valor da métrica em relação a um limite.
*   **Como funciona:** Você cria um alarme que monitora uma métrica (ex: utilização da CPU). Você define um limite (ex: > 70%) e um período (ex: por 5 minutos consecutivos).
*   **Estados do Alarme:** Um alarme tem três estados possíveis:
    *   `OK`: A métrica está dentro do limite definido.
    *   `ALARM`: A métrica excedeu o limite.
    *   `INSUFFICIENT_DATA`: Não há dados suficientes para determinar o estado do alarme.
*   **Ações:** Quando um alarme entra no estado `ALARM`, ele pode acionar ações, como:
    *   Enviar uma notificação para um tópico do Amazon SNS (Simple Notification Service).
    *   Acionar uma ação do EC2 Auto Scaling (para adicionar ou remover instâncias).
    *   Parar, terminar ou reiniciar uma instância EC2.

#### 3. CloudWatch Logs
*   **O que são:** Permitem que você centralize os logs de todos os seus sistemas, aplicações e serviços da AWS em um único serviço altamente escalável.
*   **Como funciona:**
    *   **Agente do CloudWatch:** Você pode instalar um agente em suas instâncias EC2 para enviar logs do sistema operacional e de aplicações para o CloudWatch Logs.
    *   **Integração de Serviços:** Muitos serviços da AWS podem ser configurados para enviar seus logs diretamente para o CloudWatch Logs (ex: logs de fluxo da VPC, logs do Lambda, logs do Route 53).
*   **Componentes:**
    *   **Log Events:** Um registro de alguma atividade registrada pela aplicação ou recurso.
    *   **Log Streams:** Uma sequência de eventos de log que compartilham a mesma fonte.
    *   **Log Groups:** Um grupo de fluxos de log que compartilham as mesmas configurações de retenção, monitoramento e controle de acesso.
*   **Recursos:**
    *   **Filtros de Métrica (Metric Filters):** Permitem pesquisar e corresponder a termos ou padrões em seus eventos de log e transformá-los em métricas do CloudWatch.
    *   **CloudWatch Logs Insights:** Um ambiente de consulta interativo para explorar e analisar seus dados de log.

### Benefícios do CloudWatch

*   **Observabilidade Centralizada:** Fornece um local único para monitorar todos os seus recursos da AWS.
*   **Visão Abrangente:** Coleta métricas, logs e eventos, dando uma visão completa da saúde e do desempenho da sua aplicação.
*   **Automação:** Permite criar respostas automatizadas a eventos operacionais.
*   **Flexibilidade:** Monitore recursos da AWS, recursos on-premises (via agente) e publique suas próprias métricas personalizadas.
