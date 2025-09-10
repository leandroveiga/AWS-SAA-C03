### O que é AWS Secrets Manager

O AWS Secrets Manager é um serviço de gerenciamento de segredos que ajuda você a proteger o acesso a suas aplicações, serviços e recursos de TI. Ele permite que você rotacione, gerencie e recupere facilmente credenciais de banco de dados, chaves de API e outros segredos ao longo de seu ciclo de vida. O Secrets Manager oferece uma alternativa segura para o armazenamento de segredos em texto simples no código-fonte ou em arquivos de configuração.

### Como funciona o AWS Secrets Manager

1.  **Armazenamento de um Segredo:** Você cria um "segredo" no Secrets Manager. Um segredo consiste em um conjunto de informações confidenciais, como:
    *   Credenciais de banco de dados (nome de usuário, senha, endpoint).
    *   Chaves de API para serviços de terceiros.
    *   Credenciais para outros recursos da AWS.
    *   Qualquer outra string de texto confidencial.
    Os segredos são armazenados de forma criptografada usando o AWS Key Management Service (KMS).
2.  **Recuperação do Segredo:** Sua aplicação, em vez de ter o segredo codificado, faz uma chamada de API para o Secrets Manager em tempo de execução para recuperar o segredo.
    *   **Autenticação e Autorização:** A aplicação precisa de permissões do IAM para chamar a API `GetSecretValue`. Isso garante que apenas as aplicações e usuários autorizados possam acessar segredos específicos.
    *   **Cache:** Para melhorar o desempenho e reduzir os custos, a biblioteca cliente do Secrets Manager pode armazenar em cache os segredos recuperados na memória.
3.  **Rotação Automática de Segredos:** Este é um dos recursos mais poderosos do Secrets Manager.
    *   **Como funciona:** Você pode configurar o Secrets Manager para rotacionar automaticamente os segredos em um cronograma que você define (por exemplo, a cada 30 dias). A rotação é realizada por uma **função do AWS Lambda** que o Secrets Manager invoca.
    *   **Função de Rotação:** O Lambda executa um processo de várias etapas para criar uma nova versão do segredo (uma nova senha) no serviço (por exemplo, no banco de dados) e, em seguida, atualiza o segredo no Secrets Manager com as novas credenciais.
    *   **Suporte Nativo:** O Secrets Manager tem suporte nativo para rotacionar credenciais para serviços como Amazon RDS, Redshift e DocumentDB. Para outros tipos de segredos, você pode criar uma função Lambda de rotação personalizada.

### Secrets Manager vs. AWS Systems Manager Parameter Store

A AWS oferece dois serviços para gerenciamento de segredos e configurações: Secrets Manager e Parameter Store.

| Característica | AWS Secrets Manager | AWS Systems Manager Parameter Store |
| :--- | :--- | :--- |
| **Custo** | Pago por segredo por mês e por chamada de API. | **Standard:** Gratuito. **Advanced:** Pago por parâmetro por mês. |
| **Rotação Automática** | **Sim**, recurso principal com integração Lambda. | **Não**, requer uma solução personalizada (ex: Lambda agendado). |
| **Geração de Segredos** | Pode gerar senhas aleatórias. | Não. |
| **Compartilhamento entre Contas** | Mais simples de configurar. | Requer mais configuração manual de permissões. |
| **Principal Caso de Uso** | Gerenciamento de ciclo de vida completo de segredos, especialmente credenciais que precisam de rotação (senhas de banco de dados, chaves de API). | Armazenamento de dados de configuração e segredos que não requerem rotação automática frequente. |

**Conclusão:** Use o **Secrets Manager** quando precisar de rotação automática de credenciais. Use o **Parameter Store** para armazenar dados de configuração e segredos mais estáticos, pois é mais econômico.

### Benefícios do Secrets Manager

*   **Segurança Aprimorada:** Evita o armazenamento de segredos em texto simples no código, substituindo-os por uma chamada de API em tempo de execução.
*   **Gerenciamento do Ciclo de Vida:** Automatiza a rotação de segredos, ajudando você a atender aos requisitos de segurança e conformidade.
*   **Controle de Acesso Granular:** Usa políticas do IAM para controlar rigorosamente quem e o que pode acessar os segredos.
*   **Auditoria e Monitoramento:** Integra-se com o AWS CloudTrail para registrar todas as chamadas de API e com o CloudWatch Events para notificá-lo sobre a rotação de segredos.
