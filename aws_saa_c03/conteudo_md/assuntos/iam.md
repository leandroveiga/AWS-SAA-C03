# Introdução ao AWS Identity and Access Management (IAM)
<img src="../../img/iam.png">

O AWS Identity and Access Management (IAM) é um serviço fundamental na Amazon Web Services (AWS) que permite o gerenciamento de identidades e o controle de acesso a recursos na nuvem. Aqui estão os principais conceitos relacionados ao IAM:

## Atualizações Importantes (SAA-C03 2025)
- **IAM Identity Center (ex-AWS SSO)**: Gerenciamento central de acesso federado e atribuição de permissões a contas/roles em múltiplas contas (Organizations). Use para workforce identities em vez de criar usuários IAM diretos.
- **Roles Anywhere**: Emite credenciais temporárias para workloads fora da AWS (on-prem, edge) usando certificados X.509.
- **Permission Boundaries**: Limita escopo máximo de permissões que policies anexadas podem efetivamente conceder a uma identidade.
- **Session Policies**: Refinam permissões em tempo de emissão de credenciais temporárias (STS AssumeRole) sem modificar a role.
- **Organizations & SCPs**: Service Control Policies definem guard rails – não concedem acesso, apenas restringem.
- **Access Analyzer**: Detecta acessos externos (public / cross-account) inadvertidos em recursos (S3, IAM roles, KMS, Lambda, SQS etc.).
- **Credential Report & Access Advisor**: Auditoria de uso (restringir permissions não usadas → princípio do menor privilégio iterativo).

### Padrões Recomendados
| Cenário | Recomendação |
|---------|--------------|
| Usuários humanos | Identity Center + MFA + grupos baseados em função |
| Acesso entre serviços | IAM Roles (não use chaves de acesso hardcoded) |
| Aplicações externas | OIDC Federation (Cognito / IdP externo) ou Roles Anywhere |
| Limitar escopo de automações | Permission Boundary + Tag conditions |
| Monitorar risco | CloudTrail + Access Analyzer + GuardDuty findings |

### Boas Práticas Adicionais
- Evitar uso de conta root (apenas para tarefas de break-glass raras; proteger com MFA hardware preferencial).
- Ativar *last accessed information* para refino de policies.
- Usar políticas gerenciadas por cliente (customer managed) para granularidade; evitar inline policies proliferando.
- Conditions (aws:PrincipalOrgID, aws:MultiFactorAuthPresent, s3:prefix) ajudam a restringir.

### Perguntas Típicas de Prova
1. Limitar developer para criar apenas roles que não excedam ações definidas → Permission Boundary.
2. Detectar bucket tornado público → Access Analyzer.
3. Consolidar login workforce múltiplas contas → IAM Identity Center.
4. Acesso temporário máquina on-prem sem expor chave longa → Roles Anywhere.
5. Impedir que qualquer conta crie recurso fora das regiões aprovadas → SCP em Organizations.


## Usuários, Grupos e Políticas
<img src="../../img/user_group_roles.png">

- **Usuários**: Representam pessoas ou serviços que interagem com a AWS. Cada usuário possui suas próprias credenciais de login.
  <img src="../../img/user.webp">

- **Grupos**: São coleções lógicas de usuários. As políticas são associadas a grupos para conceder permissões.
  <img src="../../img/group.png">

- **Políticas**: Definem as permissões que os usuários e grupos têm. Elas são escritas em JSON e podem ser anexadas a usuários, grupos ou recursos.
  <img src="../../img/role.png">

## Processo de Autenticação
  <img src="../../img/autenticacao.png">

- A autenticação no IAM envolve a verificação da identidade de um usuário ou serviço. Isso pode ser feito por meio de senhas, chaves de acesso, tokens ou outras formas de autenticação.

## MFA (Autenticação de Multifator)
  <img src="../../img/mfa.png">

- O MFA adiciona uma camada extra de segurança, exigindo que os usuários forneçam duas ou mais formas de identificação antes de acessar recursos críticos.

## Utilizando o Security Token Service (STS)
  <img src="../../img/sts.webp">

- O STS permite a geração de tokens temporários que concedem acesso aos recursos da AWS. Isso é útil para cenários de acesso temporário.

## Políticas de Recurso e Identidade

- As políticas de recurso controlam o acesso a recursos específicos, como buckets S3 ou instâncias EC2.

- As políticas de identidade controlam o que os usuários e grupos podem fazer no IAM, como criar usuários ou definir políticas.

## Estrutura das Políticas
  <img src="../../img/police_exemplo.png">

- As políticas são estruturadas em JSON e contêm elementos como "Effect" (Permitir ou Negar), "Action" (Ação permitida) e "Resource" (Recurso afetado).

## Boas Práticas para o IAM

- Princípio do menor privilégio: Conceda apenas as permissões necessárias para realizar uma tarefa específica.

- Rotação de credenciais: Faça a rotação regular de senhas e chaves de acesso para aumentar a segurança.

- Auditoria e monitoramento: Utilize ferramentas como AWS CloudTrail para rastrear atividades e revisar registros regularmente.

- Implemente MFA sempre que possível para proteger contas críticas.

- Siga as melhores práticas de segurança da AWS para manter sua infraestrutura segura.
