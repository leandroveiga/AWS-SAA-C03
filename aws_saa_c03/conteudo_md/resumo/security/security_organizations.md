### O que é AWS Organizations

O AWS Organizations é um serviço de gerenciamento de contas que permite consolidar várias contas da AWS em uma organização que você cria e gerencia centralmente. O Organizations inclui recursos de gerenciamento de contas e faturamento consolidado, permitindo que você atenda melhor às necessidades orçamentárias, de segurança e de conformidade de sua empresa.

### Como funciona o AWS Organizations

1.  **Criação da Organização:** Você escolhe uma conta da AWS para ser a **conta de gerenciamento (management account)**. Esta conta é usada para criar e gerenciar a organização.
2.  **Estrutura da Organização:**
    *   **Raiz (Root):** O contêiner pai para todas as contas em sua organização.
    *   **Unidades Organizacionais (Organizational Units - OUs):** Você pode organizar suas contas em uma hierarquia usando OUs. Uma OU é um contêiner para contas e pode conter outras OUs, permitindo criar uma estrutura que se assemelhe à da sua empresa (ex: OUs por departamento, por ambiente - prod/dev, ou por requisito de conformidade).
    *   **Contas Membro (Member Accounts):** As contas da AWS que fazem parte da sua organização, além da conta de gerenciamento.
3.  **Políticas de Controle de Serviço (Service Control Policies - SCPs):**
    *   **O que são:** O recurso central de governança do Organizations. As SCPs oferecem controle central sobre as permissões para todas as contas em sua organização. Elas funcionam como uma "cerca de proteção" (guardrail).
    *   **Como funcionam:** Uma SCP é uma política baseada em JSON, semelhante a uma política do IAM, que especifica os serviços e ações da AWS que os usuários e papéis podem usar nas contas afetadas. Você pode anexar SCPs à raiz da organização, a OUs ou a contas individuais.
    *   **Importante:** As SCPs **não concedem permissões**. Elas apenas definem os limites máximos de permissão. As permissões reais ainda devem ser concedidas aos usuários e papéis por meio de políticas do IAM. A permissão efetiva é a interseção entre o que a SCP permite e o que a política do IAM permite. Por exemplo, se uma SCP nega o acesso a `ec2:RunInstances`, ninguém em uma conta afetada poderá iniciar instâncias EC2, mesmo que tenha uma política do IAM `AdministratorAccess`.

### Recursos e Benefícios Principais

*   **Gerenciamento Centralizado:** Gerencie todas as suas contas da AWS como uma única unidade a partir da conta de gerenciamento.
*   **Faturamento Consolidado (Consolidated Billing):** Receba uma única fatura para todas as contas em sua organização. Você também pode se beneficiar de preços combinados (por exemplo, o uso de todas as contas é agregado para se qualificar para descontos por volume) e compartilhar instâncias reservadas entre as contas.
*   **Governança e Controle com SCPs:** Aplique políticas de segurança e conformidade de forma consistente em toda a sua organização, garantindo que as contas permaneçam dentro das diretrizes de controle de acesso da sua empresa.
*   **Criação de Contas Programática:** Automatize a criação de novas contas da AWS usando APIs.
*   **Compartilhamento de Recursos:** Facilita o compartilhamento de recursos, como licenças e instâncias reservadas, entre as contas.
*   **Integração com Serviços da AWS:** Muitos serviços da AWS se integram ao Organizations para operar em todas as contas da sua organização (ex: AWS CloudFormation StackSets, AWS Config, AWS Backup).

### Por que usar Múltiplas Contas?

Usar uma estratégia de múltiplas contas com o AWS Organizations é uma prática recomendada pela AWS por vários motivos:
*   **Isolamento de Segurança:** Isola cargas de trabalho e dados. Um incidente de segurança em uma conta não afeta diretamente as outras.
*   **Limites de Serviço:** Separa as cargas de trabalho para evitar que uma aplicação de alto tráfego consuma todos os limites de serviço da conta, impactando outras.
*   **Organização de Faturamento:** Facilita o rastreamento de custos por projeto, departamento ou ambiente.
*   **Requisitos de Conformidade:** Permite isolar dados e cargas de trabalho que estão sujeitos a requisitos regulatórios específicos, como HIPAA ou PCI DSS.
