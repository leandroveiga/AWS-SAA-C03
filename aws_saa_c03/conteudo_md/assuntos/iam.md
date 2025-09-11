# AWS Identity and Access Management (IAM)
<img src="../../img/iam.png">

O **AWS Identity and Access Management (IAM)** é o serviço que permite gerenciar o acesso aos serviços e recursos da AWS de forma segura. Com o IAM, você pode criar e gerenciar usuários e grupos da AWS e usar permissões para permitir e negar o acesso a recursos da AWS. O IAM é um serviço global e um pilar fundamental da segurança na AWS.

O IAM opera com base em três conceitos principais: **Principals**, **Authentication**, e **Authorization**.

---

## 1. Principals (Quem pode agir?)
<img src="../../img/user_group_roles.png">

Um *principal* é uma pessoa ou aplicação que pode fazer uma solicitação para uma ação ou operação em um recurso da AWS.

-   **Usuário Raiz (Root User):** A identidade criada quando você abre sua conta AWS. Possui acesso completo a todos os serviços e recursos. **Boa prática:** Não use o usuário root para tarefas diárias. Habilite a Autenticação Multi-Fator (MFA) para ele e guarde as credenciais em um local seguro.

-   **Usuários IAM (IAM Users):**
    <img src="../../img/user.webp">
    Uma entidade que você cria na AWS para representar a pessoa ou aplicação que a utiliza para interagir com a AWS. Um usuário IAM consiste em um nome e credenciais (senha para o console e/ou chaves de acesso para a CLI/SDK).

-   **Grupos IAM (IAM Groups):**
    <img src="../../img/group.png">
    Uma coleção de usuários IAM. Os grupos permitem que você especifique permissões para múltiplos usuários, o que pode facilitar o gerenciamento de permissões. Um usuário pode pertencer a múltiplos grupos.

-   **Funções IAM (IAM Roles):**
    <img src="../../img/role.png">
    Uma identidade IAM que você pode criar em sua conta que tem permissões específicas. Uma role é semelhante a um usuário, mas **não possui credenciais de longo prazo** (senha ou chaves de acesso). Em vez disso, quando uma entidade (usuário ou serviço) assume uma role, ela obtém credenciais de segurança temporárias.
    -   **Caso de Uso Principal:** Delegar acesso a usuários, aplicações ou serviços que normalmente não têm acesso aos seus recursos da AWS. Por exemplo, permitir que uma instância EC2 acesse um bucket S3 sem armazenar chaves de acesso na instância.

---

## 2. Autenticação (Quem é você?)
<img src="../../img/autenticacao.png">
A autenticação é o processo de verificar a identidade de um principal.

-   **Senha:** Para acesso ao Console de Gerenciamento da AWS.
-   **Chaves de Acesso (Access Keys):** Para acesso programático via AWS CLI, SDK ou API.
-   **Autenticação Multi-Fator (MFA):**
    <img src="../../img/mfa.png">
    Uma camada extra de segurança. Após fornecer a senha ou chave, o usuário deve fornecer um segundo fator de autenticação de um dispositivo MFA (físico ou virtual). **Boa prática:** Habilite o MFA para o usuário root e para todos os usuários IAM com permissões sensíveis.

---

## 3. Autorização (O que você pode fazer?)
A autorização é o processo de determinar quais permissões um principal autenticado possui. Isso é feito através de **Políticas IAM**.

### Políticas IAM (IAM Policies)
<img src="../../img/police_exemplo.png">
Uma política é um documento JSON que define permissões. Ela especifica quais ações são permitidas ou negadas, em quais recursos.

#### Estrutura de uma Política JSON:
-   **Effect:** `Allow` (Permitir) ou `Deny` (Negar).
-   **Action:** A ação do serviço que será permitida ou negada (ex: `s3:GetObject`, `ec2:StartInstances`).
-   **Resource:** O recurso da AWS ao qual a ação se aplica, identificado por um ARN (Amazon Resource Name).
-   **Condition (Opcional):** Condições para que a política entre em vigor (ex: `aws:SourceIp`, `aws:CurrentTime`).

#### Tipos de Políticas:
1.  **Políticas Baseadas em Identidade (Identity-Based Policies):** Anexadas a um principal IAM (usuário, grupo ou role). Elas definem o que *aquela identidade* pode fazer.
2.  **Políticas Baseadas em Recurso (Resource-Based Policies):** Anexadas a um recurso (ex: um bucket S3, uma fila SQS). Elas definem quem tem permissão para acessar *aquele recurso*.

---

## AWS Security Token Service (STS)
<img src="../../img/sts.webp">

O **STS** é um serviço web que permite solicitar credenciais temporárias com privilégios limitados para usuários IAM ou para usuários que você autentica (usuários federados). É o serviço que gera as credenciais temporárias quando uma *Role* é assumida.

## AWS IAM Identity Center (antigo AWS SSO)

O **IAM Identity Center** é o serviço recomendado para gerenciar o acesso humano a múltiplas contas da AWS e aplicações na nuvem. Ele simplifica o gerenciamento de acesso, fornecendo um local central para criar ou conectar identidades de usuários e atribuir-lhes acesso.

-   **Como funciona:** Em vez de criar usuários IAM em cada conta, você gerencia seus usuários e grupos em um único local. Os usuários fazem login em um portal central e, a partir daí, acessam as contas e aplicações da AWS para as quais receberam permissões.
-   **Fonte de Identidade:** Pode usar seu próprio provedor de identidade (como Active Directory, Okta, Azure AD) ou o diretório do próprio Identity Center.
-   **Permission Sets:** As permissões são definidas em "Conjuntos de Permissões" (que são essencialmente abstrações sobre as políticas do IAM) e atribuídas a usuários ou grupos para contas específicas.
-   **Vantagem:** Centraliza o gerenciamento de usuários, simplifica o login (Single Sign-On) e melhora a postura de segurança ao evitar a proliferação de usuários IAM e chaves de acesso de longo prazo.

---

## Melhores Práticas de Segurança do IAM

-   **Princípio do Menor Privilégio:** Conceda apenas as permissões mínimas necessárias para realizar uma tarefa.
-   **Use Roles para Aplicações:** Para aplicações executadas em instâncias EC2, use IAM Roles para fornecer credenciais temporárias em vez de armazenar chaves de acesso na instância.
-   **Nunca use o Root User:** Para tarefas do dia-a-dia.
-   **Habilite MFA:** Para o usuário root e usuários privilegiados.
-   **Rotacione Credenciais:** Rotacione chaves de acesso regularmente.
-   **Use Políticas para Controlar Acesso:** Em vez de conceder permissões diretamente aos usuários, use grupos e anexe políticas a eles.
-   **Monitore a Atividade:** Use o **AWS CloudTrail** para registrar todas as chamadas de API feitas em sua conta e auditar atividades.

