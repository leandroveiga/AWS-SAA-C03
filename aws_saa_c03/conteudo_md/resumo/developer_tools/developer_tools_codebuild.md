### O que é AWS CodeBuild

O AWS CodeBuild é um serviço de integração contínua (CI) totalmente gerenciado que compila o código-fonte, executa testes e produz pacotes de software prontos para implantação. Com o CodeBuild, você não precisa provisionar, gerenciar e escalar seus próprios servidores de compilação. Ele escala continuamente e processa várias compilações em paralelo, para que suas compilações não fiquem esperando em uma fila.

### Como funciona o AWS CodeBuild

1.  **Projeto de Compilação (Build Project):** Você define um "projeto de compilação" que especifica como o CodeBuild deve executar sua compilação. As principais configurações são:
    *   **Fonte (Source):** Onde o seu código-fonte está localizado. Pode ser AWS CodeCommit, GitHub, Bitbucket ou Amazon S3.
    *   **Ambiente de Compilação (Build Environment):** O ambiente de computação que o CodeBuild usará. O CodeBuild fornece ambientes pré-configurados com sistemas operacionais (Amazon Linux, Ubuntu, Windows) e runtimes de programação (Java, Python, Node.js, etc.). Você também pode usar sua própria imagem Docker personalizada.
    *   **Buildspec:** Um arquivo de configuração em formato YAML, chamado `buildspec.yml`, que você inclui na raiz do seu código-fonte. Este arquivo informa ao CodeBuild quais comandos executar em cada fase da compilação.
    *   **Artefatos (Artifacts):** A localização onde o CodeBuild deve enviar o resultado da compilação (o artefato), que geralmente é um bucket do Amazon S3.
2.  **Arquivo `buildspec.yml`:** Este é o coração do processo de compilação. Ele é dividido em fases:
    *   `install`: Comandos para instalar dependências necessárias para a compilação.
    *   `pre_build`: Comandos a serem executados antes da compilação (ex: autenticar em um registro de contêiner).
    *   `build`: Os comandos principais para compilar seu código e executar testes de unidade.
    *   `post_build`: Comandos a serem executados após a compilação (ex: empurrar uma imagem Docker para o ECR).
    *   `artifacts`: Especifica os arquivos e diretórios a serem incluídos no artefato de compilação.
3.  **Execução da Compilação:** Quando você inicia uma compilação (manualmente ou via automação), o CodeBuild:
    *   Inicia um contêiner efêmero e isolado com base no ambiente que você especificou.
    *   Baixa o código-fonte para o contêiner.
    *   Executa os comandos definidos no arquivo `buildspec.yml`.
    *   Carrega o artefato de compilação resultante para o bucket S3 especificado.
    *   Destrói o contêiner de compilação.

### CodeBuild no Pipeline de CI/CD

O CodeBuild é a fase de "Build" e "Test" em um pipeline de CI/CD típico na AWS:

1.  **Fonte (Source):** Um desenvolvedor envia o código para o **AWS CodeCommit**.
2.  **Gatilho (Trigger):** O **AWS CodePipeline** detecta a alteração e inicia o pipeline.
3.  **Construção (Build):** O CodePipeline invoca o **AWS CodeBuild**. O CodeBuild busca o código, executa os testes definidos no `buildspec.yml` e, se tudo for bem-sucedido, cria um artefato (por exemplo, um arquivo JAR, um diretório com arquivos estáticos ou uma imagem Docker).
4.  **Implantação (Deploy):** O CodePipeline pega o artefato e o passa para o **AWS CodeDeploy** para a implantação.

### Benefícios do CodeBuild

*   **Totalmente Gerenciado:** Não há servidores de compilação para provisionar ou gerenciar.
*   **Escalonamento Contínuo:** Escala sob demanda para atender ao volume de suas compilações, executando-as em paralelo.
*   **Custo-Benefício (Pague pelo Uso):** Você é cobrado pelo tempo de computação que consome, em minutos. Você não paga por tempo ocioso do servidor de compilação.
*   **Flexibilidade:** Suporta vários runtimes e permite que você traga sua própria imagem Docker para criar ambientes de compilação personalizados.
*   **Segurança:** Cada compilação é executada em um ambiente novo e isolado, e se integra com o AWS KMS para criptografar os artefatos de compilação.
*   **Extensibilidade:** Pode ser invocado via API, CLI ou como parte de um pipeline do AWS CodePipeline.
