### O que é AWS IAM (Identity and Access Management)

O AWS Identity and Access Management (IAM) é um serviço da web que ajuda você a controlar de forma segura o acesso aos recursos da AWS. Você usa o IAM para controlar quem é autenticado (fez login) e autorizado (tem permissões) a usar os recursos. O IAM é um recurso da sua conta da AWS oferecido sem custo adicional.

### Como funciona o AWS IAM

O IAM permite gerenciar o acesso à AWS através de quatro componentes principais:

1.  **Usuários (Users):** Uma entidade que você cria na AWS para representar a pessoa ou aplicação que a utiliza para interagir com os recursos da AWS. Um usuário do IAM consiste em um nome e credenciais.
    *   **Credenciais:** Podem ser uma senha para acesso ao Console de Gerenciamento da AWS e/ou chaves de acesso (ID da chave de acesso e chave de acesso secreta) para uso programático via API ou CLI.
2.  **Grupos (Groups):** Uma coleção de usuários do IAM. Os grupos permitem que você especifique permissões para vários usuários, o que pode facilitar o gerenciamento das permissões. Por exemplo, você pode ter um grupo chamado `Admins` e dar a esse grupo os tipos de permissões que os administradores normalmente precisam.
3.  **Políticas (Policies):** O núcleo do IAM. Uma política é um documento (em formato JSON) que define explicitamente as permissões. As políticas são anexadas a usuários, grupos ou papéis.
    *   **Estrutura da Política:** Uma política consiste em uma ou mais declarações. Cada declaração inclui:
        *   `Effect`: `Allow` (Permitir) ou `Deny` (Negar).
        *   `Action`: A ação do serviço que é permitida ou negada (ex: `s3:GetObject`).
        *   `Resource`: O recurso da AWS ao qual a ação se aplica (ex: um bucket S3 específico).
        *   `Condition` (Opcional): Circunstâncias sob as quais a política concede permissão.
4.  **Papéis (Roles):** Uma identidade do IAM que você pode criar em sua conta e que tem permissões específicas. Um papel é semelhante a um usuário, pois é uma identidade da AWS com políticas de permissão. No entanto, em vez de ser associado exclusivamente a uma pessoa, um papel destina-se a ser assumido por qualquer pessoa que precise dele.
    *   **Como funciona:** Um papel não tem credenciais de longo prazo, como senhas ou chaves de acesso. Em vez disso, quando um usuário ou serviço assume um papel, ele fornece credenciais de segurança temporárias para a sessão.
    *   **Casos de uso:**
        *   Permitir que um serviço da AWS (como o EC2) acesse outro (como o S3).
        *   Conceder acesso entre contas da AWS.
        *   Federação de identidade com um provedor de identidade corporativo (como Active Directory).

### Melhores Práticas de IAM

*   **Não use o usuário root:** Crie um usuário administrativo do IAM para tarefas diárias.
*   **Princípio do menor privilégio:** Conceda apenas as permissões mínimas necessárias para realizar uma tarefa.
*   **Use grupos para atribuir permissões a usuários:** Facilita o gerenciamento.
*   **Use papéis para aplicações executadas em instâncias EC2:** É mais seguro do que armazenar chaves de acesso na instância.
*   **Habilite a Autenticação Multifator (MFA):** Adiciona uma camada extra de segurança para o usuário root e usuários privilegiados.
*   **Rotacione as credenciais regularmente:** Altere senhas e chaves de acesso periodicamente.

### Benefícios do IAM

*   **Controle de Acesso Granular:** Permite definir permissões detalhadas para usuários e recursos.
*   **Segurança Aprimorada:** Centraliza o gerenciamento de acesso e melhora a postura de segurança da sua conta.
*   **Federação de Identidade:** Integra-se com seus sistemas de identidade existentes.
*   **Gratuito:** Não há custo adicional para usar o IAM.
