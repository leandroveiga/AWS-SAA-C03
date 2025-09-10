### O que é AWS CloudFormation

O AWS CloudFormation é um serviço que oferece uma maneira fácil de modelar um conjunto de recursos relacionados da AWS e de terceiros, provisioná-los de forma rápida e consistente e gerenciá-los ao longo de seus ciclos de vida. Ele permite que você use uma linguagem de programação ou um arquivo de texto simples para modelar e provisionar todos os recursos necessários para suas aplicações em todas as regiões e contas. Isso é conhecido como Infraestrutura como Código (Infrastructure as Code - IaC).

### Como funciona o AWS CloudFormation

1.  **Criação do Modelo (Template):** Você cria um modelo que descreve todos os recursos da AWS que você deseja (como instâncias Amazon EC2, bancos de dados Amazon RDS ou grupos de segurança do IAM). Os modelos são arquivos de texto formatados em JSON ou YAML.
2.  **Estrutura do Modelo:** Um modelo do CloudFormation tem várias seções principais:
    *   `Parameters` (Opcional): Valores de entrada que você pode passar para o modelo no momento da criação da pilha (ex: tipo de instância, senhas).
    *   `Mappings` (Opcional): Um mapa de chaves e valores que você pode usar para especificar valores condicionais (ex: mapear uma região para uma AMI específica).
    *   `Resources` (Obrigatório): A seção principal onde você declara os recursos da AWS que deseja criar e configurar.
    *   `Outputs` (Opcional): Descreve os valores que você deseja retornar após a criação da pilha (ex: o URL de um site ou o ARN de um recurso).
3.  **Criação da Pilha (Stack):** Você faz o upload do seu modelo para o CloudFormation e cria uma "pilha" (stack). Uma pilha é um conjunto de recursos da AWS que você gerencia como uma única unidade.
4.  **Provisionamento de Recursos:** O CloudFormation lê seu modelo e faz as chamadas de API necessárias para provisionar e configurar os recursos em sua conta da AWS na ordem correta e com as dependências apropriadas. Por exemplo, ele criará uma VPC antes de criar uma sub-rede dentro dela.
5.  **Gerenciamento da Pilha:**
    *   **Atualização:** Se você precisar alterar os recursos em sua pilha, você modifica o modelo e atualiza a pilha. O CloudFormation identifica o que mudou e faz as alterações necessárias.
    *   **Exclusão:** Quando você exclui uma pilha, o CloudFormation exclui todos os recursos que foram criados como parte dela.

### Recursos Principais

*   **Change Sets (Conjuntos de Alterações):** Antes de atualizar uma pilha, você pode gerar um conjunto de alterações. Isso mostra um resumo das alterações propostas que o CloudFormation fará, permitindo que você revise as alterações antes de executá-las.
*   **StackSets:** Permitem que você crie, atualize ou exclua pilhas em várias contas e regiões da AWS com uma única operação.
*   **Drift Detection (Detecção de Desvio):** Permite detectar se a configuração de uma pilha se desviou da configuração definida em seu modelo. Isso ajuda a identificar alterações feitas nos recursos fora do gerenciamento do CloudFormation.
*   **Custom Resources (Recursos Personalizados):** Permitem que você escreva lógica de provisionamento personalizada (usando AWS Lambda) e a inclua em seu modelo do CloudFormation.

### Benefícios do CloudFormation

*   **Infraestrutura como Código (IaC):** Trate sua infraestrutura como código, permitindo que você a versione, revise e reutilize.
*   **Automação e Consistência:** Automatiza o provisionamento de infraestrutura, eliminando erros manuais e garantindo implantações consistentes e repetíveis.
*   **Gerenciamento Simplificado:** Gerencia a criação, atualização e exclusão de um grupo de recursos como uma única unidade (a pilha).
*   **Visibilidade e Controle:** O modelo serve como uma única fonte de verdade para a configuração de seus recursos.
*   **Gratuito:** Não há custo adicional pelo CloudFormation. Você paga apenas pelos recursos da AWS que ele cria.
