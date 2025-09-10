### O que é AWS CodeDeploy

O AWS CodeDeploy é um serviço de implantação totalmente gerenciado que automatiza as implantações de software em uma variedade de serviços de computação, como instâncias Amazon EC2, AWS Fargate, AWS Lambda e servidores on-premises. O CodeDeploy facilita o lançamento rápido de novos recursos, ajuda a evitar o tempo de inatividade durante a implantação da aplicação e lida com a complexidade da atualização de suas aplicações.

### Como funciona o AWS CodeDeploy

O CodeDeploy automatiza as implantações por meio de um processo controlado.

1.  **Revisão da Aplicação (Application Revision):** É um arquivo que contém o código-fonte ou os artefatos a serem implantados, juntamente com um arquivo de especificação da aplicação (AppSpec).
2.  **Arquivo AppSpec (Application Specification File):** Este é o arquivo de configuração principal para o CodeDeploy, em formato YAML. Ele define tudo o que o CodeDeploy precisa saber para implantar sua aplicação:
    *   **`files`:** Especifica os arquivos de origem (do seu artefato) e para onde eles devem ser copiados no destino.
    *   **`hooks`:** O mais importante. Define os scripts ou programas a serem executados em diferentes "ganchos" do ciclo de vida da implantação. Por exemplo:
        *   `BeforeInstall`: Executar um script para fazer backup da versão atual.
        *   `AfterInstall`: Executar um script para configurar permissões de arquivo.
        *   `ApplicationStart`: Iniciar seu serviço ou aplicação.
        *   `ValidateService`: Executar um teste para garantir que a aplicação foi implantada e está funcionando corretamente.
3.  **Grupo de Implantação (Deployment Group):** Um conjunto de instâncias de destino individuais (por exemplo, instâncias EC2 marcadas com uma tag específica, um serviço ECS ou uma função Lambda).
4.  **Configuração de Implantação (Deployment Configuration):** Define a estratégia de implantação, ou seja, como a implantação deve proceder. As estratégias controlam a velocidade da implantação e o nível de disponibilidade durante a atualização.

### Estratégias de Implantação

O CodeDeploy oferece várias estratégias de implantação integradas:

*   **One-at-a-Time (Um de Cada Vez):** Implanta em uma instância de cada vez. Se uma implantação falhar, as instâncias restantes não são atualizadas. É lento, mas muito seguro.
*   **Half-at-a-Time (Metade de Cada Vez):** Implanta em até metade das instâncias de cada vez.
*   **All-at-Once (Tudo de Uma Vez):** Implanta em todas as instâncias simultaneamente. É a maneira mais rápida, mas resulta em tempo de inatividade.
*   **Canary (Canário):** O tráfego é deslocado para as novas versões em duas incrementos. Você pode escolher um percentual de tráfego a ser deslocado no primeiro incremento e, em seguida, o restante no segundo.
*   **Linear:** O tráfego é deslocado para as novas versões em incrementos percentuais iguais com um intervalo de tempo igual entre cada incremento.
*   **Blue/Green (Azul/Verde) - para EC2/On-Premises:** O tráfego é deslocado de suas instâncias originais ("ambiente azul") para um novo conjunto de instâncias de substituição ("ambiente verde"). O CodeDeploy provisiona as novas instâncias e, após a implantação bem-sucedida, pode encerrar as instâncias antigas. Isso oferece a maior segurança e zero tempo de inatividade.

### Tipos de Implantação

*   **EC2/On-Premises:** Implanta aplicações em instâncias EC2 ou servidores locais. Requer que o **Agente do CodeDeploy** esteja instalado e em execução nos destinos.
*   **AWS Lambda:** Implanta novas versões de suas funções do Lambda. Suporta estratégias de implantação gradual (Canary e Linear) para deslocar o tráfego para a nova versão ao longo do tempo.
*   **Amazon ECS:** Implanta uma nova versão de uma definição de tarefa como um serviço do Amazon ECS. Suporta implantações blue/green.

### Benefícios do CodeDeploy

*   **Implantações Automatizadas:** Reduz a necessidade de processos manuais propensos a erros.
*   **Minimização do Tempo de Inatividade:** As estratégias de implantação, como blue/green e atualizações contínuas, ajudam a manter suas aplicações disponíveis durante as atualizações.
*   **Controle Centralizado:** Fornece um local central para gerenciar e monitorar o status de suas implantações.
*   **Reversão Automática (Automatic Rollback):** Pode ser configurado para reverter automaticamente uma implantação se alarmes do CloudWatch forem acionados ou se houver muitas falhas.
*   **Fácil de Adotar:** Funciona com qualquer aplicação e fornece um modelo de ciclo de vida de implantação claro através do arquivo AppSpec.
