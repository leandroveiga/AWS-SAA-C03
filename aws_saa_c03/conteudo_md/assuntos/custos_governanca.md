# Gerenciamento de Custos e Governança

Além do AWS Organizations e Control Tower, a AWS fornece ferramentas essenciais para monitorar, analisar e otimizar os custos, além de garantir a conformidade com as melhores práticas.

## 1. AWS Cost Explorer

-   **O que é:** Uma ferramenta que permite visualizar, entender e gerenciar seus custos e uso da AWS ao longo do tempo.
-   **Funcionalidades:**
    -   Cria gráficos personalizados para analisar dados de custo e uso.
    -   Filtra e agrupa dados por tags, serviços, contas, etc.
    -   Fornece previsões de custos para os próximos meses.

## 2. AWS Budgets

-   **O que é:** Permite definir orçamentos personalizados para rastrear seus custos e uso.
-   **Funcionalidades:**
    -   **Alertas:** Notifica você (via SNS) quando seu custo ou uso excede (ou está previsto para exceder) o valor orçado.
    -   **Ações de Orçamento:** Pode acionar ações automaticamente (ex: aplicar uma política do IAM ou SCP) para restringir permissões quando um orçamento é ultrapassado, ajudando a evitar gastos excessivos.

## 3. AWS Trusted Advisor

-   **O que é:** Um serviço de orientação em tempo real que ajuda a seguir as melhores práticas da AWS. Ele inspeciona seu ambiente AWS e faz recomendações.
-   **Cinco Pilares:**
    1.  **Otimização de Custos:** Identifica recursos ociosos (ex: instâncias EC2 com baixa utilização, Elastic IPs não associados).
    2.  **Performance:** Verifica a utilização de serviços para garantir alta performance (ex: snapshots de EBS antigos).
    3.  **Segurança:** Recomendações para melhorar a segurança (ex: buckets S3 com acesso público, falta de MFA no usuário root).
    4.  **Tolerância a Falhas:** Sugestões para aumentar a resiliência (ex: implantações RDS não Multi-AZ, falta de backups).
    5.  **Limites de Serviço (Service Quotas):** Verifica o uso em relação aos limites da conta.
-   **Níveis de Acesso:** O plano Basic Support (gratuito) oferece acesso a verificações de segurança e limites de serviço. Planos pagos (Developer, Business, Enterprise) oferecem acesso a todas as verificações.
