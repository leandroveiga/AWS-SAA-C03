# Gerenciamento de Custos e Governança

Gerenciar custos e estabelecer uma governança robusta são pilares fundamentais para o sucesso na nuvem AWS. A AWS oferece um conjunto de ferramentas para monitorar, otimizar custos e governar ambientes com múltiplas contas.

## 1. AWS Cost Explorer

-   **O que é:** O AWS Cost Explorer é uma ferramenta poderosa e gratuita que fornece uma interface visual para explorar, analisar e gerenciar seus custos e uso da AWS. Ele permite que você mergulhe fundo nos seus gastos, identifique tendências, aponte anomalias de custo e descubra oportunidades de economia.

-   **Principais Funcionalidades e Casos de Uso:**
    -   **Visualização e Análise Detalhada:**
        -   **Relatórios Intuitivos:** Oferece uma variedade de gráficos e relatórios prontos para uso que mostram seus gastos ao longo do tempo. Você pode visualizar dados por mês, dia ou até mesmo por hora.
        -   **Filtragem e Agrupamento (Filtering & Grouping):** Esta é uma das funcionalidades mais importantes. Você pode filtrar e agrupar seus custos por múltiplas dimensões, como:
            -   **Serviço AWS:** (ex: EC2, S3, RDS) para ver qual serviço está consumindo mais recursos.
            -   **Conta-Membro:** (em uma AWS Organization) para identificar qual departamento ou projeto está gastando mais.
            -   **Região:** Para entender a distribuição geográfica dos seus custos.
            -   **Tags de Custo:** Essencial para alocação de custos. Se você taguear seus recursos (ex: `cost-center:marketing`, `project:x`), pode filtrar os custos exatos de um centro de custo ou projeto específico.
            -   **Tipo de Uso:** (ex: `DataTransfer-Out-Bytes`) para analisar custos específicos.
    -   **Previsão de Custos (Forecasting):**
        -   Com base no seu histórico de gastos, o Cost Explorer pode prever seus custos para os próximos 12 meses. Isso ajuda no planejamento orçamentário e a evitar surpresas na fatura.
    -   **Relatórios de Otimização de Custos:**
        -   **Recomendações de Instâncias Reservadas (RIs) e Savings Plans:** O Cost Explorer analisa seu uso de EC2 e outros serviços para recomendar a compra de RIs ou Savings Plans, que podem gerar economias significativas (até 72%) em troca de um compromisso de uso. Ele mostra exatamente quanto você poderia economizar.

-   **Comparação com AWS Budgets:**
    -   **Cost Explorer** é para **análise e exploração** (passado e futuro). É onde você vai para *entender* seus custos.
    -   **AWS Budgets** é para **controle e ação**. Você define um limite e o serviço te *alerta* ou *age* quando esse limite é atingido.

## 2. AWS Budgets

-   **O que é:** Permite definir orçamentos personalizados para rastrear seus custos e uso.
-   **Funcionalidades:**
    -   **Alertas:** Notifica você (via SNS) quando seu custo ou uso excede (ou está previsto para exceder) o valor orçado.
    -   **Ações de Orçamento:** Pode acionar ações automaticamente (ex: aplicar uma política do IAM ou SCP) para restringir permissões quando um orçamento é ultrapassado, ajudando a evitar gastos excessivos.

---

## 3. AWS Organizations e Control Tower

-   **AWS Organizations:**
    -   Permite gerenciar centralmente múltiplas contas AWS.
    -   **Faturamento Consolidado (Consolidated Billing):** Centraliza o pagamento de todas as contas e pode oferecer descontos por volume.
    -   **Unidades Organizacionais (OUs):** Permite agrupar contas para aplicar políticas de gerenciamento (ex: OUs para "Produção", "Desenvolvimento").
    -   **Políticas de Controle de Serviço (SCPs - Service Control Policies):** A funcionalidade mais poderosa. SCPs são "guardrails" que restringem as permissões que podem ser usadas nas contas-membro, mesmo para o usuário `root` da conta. Ex: "Ninguém na OU de Desenvolvimento pode lançar instâncias EC2 do tipo `large` ou superior".

-   **AWS Control Tower:**
    -   É um serviço que automatiza a configuração de um ambiente AWS seguro e com várias contas, chamado de "landing zone".
    -   Ele usa outros serviços por baixo dos panos (como Organizations, IAM, Config) para aplicar as melhores práticas da AWS de forma automatizada.
    -   **Guardrails:** Implementa regras de alto nível para governança (ex: "detectar desvio de configuração", "proibir alterações em regras do CloudTrail").
