# Serviços Avançados e Estratégias de Recuperação de Desastres (DR)

Este tópico aborda serviços e conceitos cruciais para a excelência operacional, segurança avançada e resiliência de arquiteturas na AWS.

## 1. AWS Systems Manager (SSM)

O AWS Systems Manager (SSM) é o centro de operações da AWS, fornecendo uma interface de usuário unificada para visualizar dados operacionais de vários serviços da AWS e automatizar tarefas operacionais em seus recursos. Ele é fundamental para o gerenciamento seguro e em escala de instâncias EC2 e servidores on-premises.

### Principais Funcionalidades:

-   **Session Manager:**
    -   **O que é:** Permite gerenciar suas instâncias EC2 (Linux ou Windows) por meio de um shell interativo baseado em navegador ou pela AWS CLI.
    -   **Principal Vantagem:** **Elimina a necessidade de abrir portas de entrada (como a porta 22 para SSH ou 3389 para RDP) e de gerenciar chaves SSH.** O acesso é controlado via políticas do IAM, e todas as sessões podem ser logadas e auditadas no CloudTrail e CloudWatch Logs. É a maneira moderna e segura de acessar instâncias.

-   **Parameter Store:**
    -   **O que é:** Fornece armazenamento seguro e hierárquico para gerenciamento de configurações e segredos.
    -   **Dados que podem ser armazenados:**
        -   **Strings de texto plano:** URLs de banco de dados, endpoints de API.
        -   **Segredos (SecureStrings):** Senhas, chaves de API, que são criptografados usando o AWS Key Management Service (KMS).
    -   **Custo:** O armazenamento padrão é gratuito.
    -   **Uso:** Aplicações podem recuperar esses parâmetros em tempo de execução, evitando que segredos sejam codificados diretamente no código-fonte.

-   **Patch Manager:**
    -   **O que é:** Automatiza o processo de aplicação de patches em grandes grupos de instâncias EC2 ou on-premises para atualizações de segurança e outras.
    -   **Como funciona:** Você pode definir janelas de manutenção e "baselines" de patches (regras para aprovação automática de patches) para garantir que seus sistemas estejam sempre atualizados.

-   **Run Command:**
    -   **O que é:** Permite executar comandos remotamente em suas instâncias de forma segura e em escala, sem a necessidade de login via SSH ou RDP.
    -   **Caso de uso:** Instalar software, executar scripts ou aplicar configurações em um grande número de instâncias simultaneamente.

---

## 2. AWS Secrets Manager

O AWS Secrets Manager é um serviço dedicado a proteger e gerenciar segredos, como senhas de banco de dados, chaves de API e outras credenciais.

-   **Principais Funcionalidades:**
    -   **Rotação Automática de Segredos:** A principal vantagem sobre o SSM Parameter Store. O Secrets Manager pode rotacionar segredos automaticamente e de forma nativa para serviços como Amazon RDS, Redshift e DocumentDB. Isso aumenta significativamente a postura de segurança.
    -   **Criptografia:** Os segredos são sempre criptografados em repouso usando o AWS KMS.
    -   **Controle de Acesso:** O acesso aos segredos é controlado por políticas do IAM.
    -   **Auditoria:** Acesso aos segredos é logado no AWS CloudTrail.

-   **Secrets Manager vs. SSM Parameter Store (SecureStrings):**
    -   **Custo:** O Secrets Manager é um serviço pago por segredo por mês e por chamada de API, enquanto o SSM Parameter Store Standard é gratuito e o Advanced tem um custo menor.
    -   **Rotação de Segredos:** O Secrets Manager oferece rotação automática nativa. No SSM, a rotação precisa ser implementada de forma personalizada (geralmente com funções Lambda).
    -   **Quando usar qual:**
        -   Use **AWS Secrets Manager** quando precisar de rotação automática de credenciais, especialmente para bancos de dados AWS.
        -   Use **SSM Parameter Store** para armazenar dados de configuração, strings de conexão e segredos que não requerem rotação automática frequente, sendo uma opção mais econômica.

-   **Caso de Uso:** Armazenar a senha de um banco de dados Amazon RDS e configurá-la para ser rotacionada automaticamente a cada 30 dias, sem intervenção manual e sem que a aplicação precise ser reiniciada.

---

## 3. Estratégias de Recuperação de Desastres (DR) na AWS

Recuperação de Desastres (DR) envolve o planejamento para a recuperação de uma aplicação após um evento catastrófico, como a falha de uma região inteira da AWS. As estratégias variam em custo, complexidade, tempo de recuperação (RTO) e ponto de recuperação (RPO).

-   **RTO (Recovery Time Objective):** Quanto tempo a aplicação pode ficar offline? (Ex: 2 horas).
-   **RPO (Recovery Point Objective):** Quanta perda de dados é aceitável? (Ex: 5 minutos de dados).

### Principais Estratégias (da mais barata/lenta para a mais cara/rápida):

1.  **Backup e Restauração (Backup and Restore):**
    -   **Descrição:** Consiste em fazer backups regulares dos seus dados (ex: snapshots do EBS, backups do RDS, arquivos no S3) e, em caso de desastre, restaurar esses backups em uma nova região.
    -   **RTO:** Alto (horas a dias).
    -   **RPO:** Alto (minutos a horas, dependendo da frequência do backup).
    -   **Custo:** Mais baixo.

2.  **Luz Piloto (Pilot Light):**
    -   **Descrição:** Uma versão mínima da sua infraestrutura está sempre em execução na região de DR. O núcleo de dados (ex: banco de dados) é replicado continuamente. Em caso de desastre, a infraestrutura completa (servidores de aplicação, etc.) é rapidamente provisionada em torno do núcleo já existente.
    -   **RTO:** Médio (dezenas de minutos a horas).
    -   **RPO:** Baixo (minutos).
    -   **Custo:** Baixo a médio.

3.  **Espera Morna (Warm Standby):**
    -   **Descrição:** Uma versão reduzida, mas totalmente funcional, da sua aplicação está sempre em execução na região de DR. Por exemplo, menos instâncias EC2 em um grupo de Auto Scaling. Em caso de desastre, o tráfego é redirecionado para a região de DR, e a infraestrutura é escalada para a capacidade total.
    -   **RTO:** Baixo (minutos).
    -   **RPO:** Muito baixo (segundos a minutos).
    -   **Custo:** Médio a alto.

4.  **Multi-Site Ativo-Ativo (Multi-site Active-Active):**
    -   **Descrição:** A aplicação é executada simultaneamente em várias regiões, e o tráfego é distribuído entre elas (ex: usando o Route 53). Se uma região falhar, o tráfego é automaticamente redirecionado para as regiões saudáveis.
    -   **RTO:** Quase zero.
    -   **RPO:** Quase zero.
    -   **Custo:** Mais alto.
