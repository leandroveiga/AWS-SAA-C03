### O que é AWS CloudTrail

O AWS CloudTrail é um serviço da AWS que ajuda você a habilitar a governança, a conformidade, a auditoria operacional e a auditoria de risco de sua conta da AWS. Com o CloudTrail, você pode registrar, monitorar continuamente e reter a atividade da conta relacionada a ações em toda a sua infraestrutura da AWS. O CloudTrail fornece um histórico de eventos de chamadas de API da AWS para sua conta, incluindo chamadas feitas por meio do Console de Gerenciamento da AWS, SDKs da AWS, ferramentas de linha de comando e outros serviços da AWS.

### Como funciona o AWS CloudTrail

1.  **Registro de Atividade:** O CloudTrail está sempre ativo em sua conta da AWS e registra a atividade da conta quando ela é criada. Ele captura informações importantes sobre cada chamada de API, incluindo:
    *   Quem fez a chamada (identidade do IAM).
    *   Quando a chamada foi feita (data e hora).
    *   De qual endereço IP a chamada foi feita.
    *   Qual ação foi realizada (ex: `ec2:RunInstances`).
    *   Quais recursos foram afetados.
    *   Quais parâmetros foram usados na solicitação.
2.  **Histórico de Eventos (Event History):** O CloudTrail armazena os últimos 90 dias de atividade da conta no "Histórico de Eventos", que pode ser visualizado e pesquisado no Console de Gerenciamento da AWS. Isso é útil para solucionar problemas operacionais recentes.
3.  **Trilhas (Trails):** Para um registro de longo prazo e recursos de auditoria mais avançados, você cria uma "trilha". Uma trilha permite que o CloudTrail entregue os arquivos de log de eventos para um bucket do Amazon S3 que você especificar.
    *   **Aplicação a Todas as Regiões:** Você pode configurar uma trilha para receber arquivos de log de todas as regiões da AWS, o que centraliza os logs de atividade de toda a sua conta.
    *   **Entrega de Logs:** O CloudTrail entrega os logs para o seu bucket do S3 em um formato JSON compactado, normalmente a cada 5 minutos.
4.  **Integração com CloudWatch Logs:** Você pode configurar uma trilha para enviar eventos para o Amazon CloudWatch Logs. Isso permite que você crie alarmes do CloudWatch para monitorar atividades específicas da API e receber notificações quando ocorrerem eventos críticos (por exemplo, um alarme para alterações em grupos de segurança ou desativação do próprio CloudTrail).
5.  **Validação da Integridade do Arquivo de Log:** O CloudTrail pode ser configurado para assinar digitalmente os arquivos de log que entrega, permitindo que você valide que os arquivos não foram alterados após a entrega pelo CloudTrail.

### Tipos de Eventos

*   **Eventos de Gerenciamento (Management Events):** Também conhecidos como "operações do plano de controle". Fornecem informações sobre operações de gerenciamento que são executadas em recursos em sua conta da AWS (ex: criar uma instância EC2, criar um bucket S3, criar um usuário IAM). Por padrão, as trilhas registram todos os eventos de gerenciamento.
*   **Eventos de Dados (Data Events):** Também conhecidos como "operações do plano de dados". Fornecem informações sobre as operações de recursos executadas em ou dentro de um recurso (ex: `s3:GetObject`, `s3:PutObject`, chamadas de função do Lambda). Esses eventos são de alto volume e não são registrados por padrão. Você deve habilitá-los explicitamente em sua trilha.
*   **Eventos do CloudTrail Insights:** Um recurso opcional que analisa a atividade normal de gerenciamento da sua conta para construir uma linha de base e, em seguida, identifica atividades incomuns, como picos no provisionamento de recursos ou rajadas de ações do IAM.

### Benefícios do CloudTrail

*   **Visibilidade e Auditoria de Segurança:** Rastreia a atividade do usuário e o uso da API para análise de segurança e solução de problemas. Responde a perguntas como "quem fez o quê e quando?".
*   **Conformidade:** Fornece um registro detalhado da atividade da conta, que é essencial para atender aos requisitos de conformidade.
*   **Solução de Problemas Operacionais:** Ajuda a identificar a causa raiz de problemas operacionais, mostrando as alterações recentes nos recursos.
*   **Detecção de Ameaças:** Pode ser usado com o CloudWatch e outras ferramentas para detectar e responder a atividades de conta potencialmente maliciosas ou não autorizadas.
