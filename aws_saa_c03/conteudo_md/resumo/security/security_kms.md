### O que é AWS KMS (Key Management Service)

O AWS Key Management Service (KMS) é um serviço gerenciado que facilita a criação e o controle de chaves de criptografia usadas para criptografar seus dados. O KMS é integrado a muitos outros serviços da AWS para ajudar a proteger os dados que você armazena nesses serviços. Ele usa módulos de segurança de hardware (HSMs) validados pelo FIPS 140-2 para proteger a segurança de suas chaves.

### Como funciona o AWS KMS

O KMS opera com base em um sistema de "criptografia de envelope" (envelope encryption).

1.  **Criação da Chave Mestra do Cliente (CMK):** Você cria uma chave de criptografia principal no KMS, chamada de Customer Master Key (CMK). Esta é a chave que você gerencia. Ela nunca deixa os HSMs do KMS descriptografada.
2.  **Geração da Chave de Dados (Data Key):** Quando você precisa criptografar dados em sua aplicação, você solicita ao KMS que gere uma "chave de dados" exclusiva para você a partir da sua CMK.
3.  **Criptografia de Envelope:** O KMS retorna duas versões da chave de dados:
    *   **Uma versão em texto simples (plaintext):** Você usa esta chave para criptografar seus dados localmente em sua aplicação.
    *   **Uma versão criptografada (encrypted):** Esta é a mesma chave de dados, mas criptografada pela sua CMK.
4.  **Armazenamento:** Você armazena a **chave de dados criptografada** junto com seus **dados criptografados**. A chave de dados em texto simples deve ser removida da memória o mais rápido possível.
5.  **Descriptografia:** Para descriptografar os dados, você envia a **chave de dados criptografada** de volta para o KMS. O KMS usa sua CMK para descriptografar a chave de dados e retorna a **chave de dados em texto simples** para você. Você então usa essa chave para descriptografar seus dados.

Este processo garante que a chave mestra (CMK), que pode descriptografar tudo, nunca seja exposta.

### Tipos de Chaves Mestras do Cliente (CMKs)

*   **Chaves Gerenciadas pela AWS (AWS Managed Keys):** São CMKs em sua conta que são criadas, gerenciadas e usadas em seu nome por um serviço da AWS integrado ao KMS (ex: S3, EBS, RDS). Você pode visualizar essas chaves, mas não pode gerenciá-las diretamente.
*   **Chaves Gerenciadas pelo Cliente (Customer Managed Keys):** São CMKs que você cria, possui e gerencia. Você tem controle total sobre essas chaves, incluindo a definição de suas políticas de acesso, rotação e desativação.
*   **Chaves Importadas (Imported Keys):** Você pode importar seu próprio material de chave de sua infraestrutura de gerenciamento de chaves on-premises para o KMS e usá-lo com os serviços da AWS.
*   **Custom Key Store (usando AWS CloudHSM):** Permite que você combine o controle de um cluster CloudHSM com a integração e a facilidade de uso do KMS. Você pode criar suas chaves KMS em um cluster CloudHSM que você controla.

### Integração com Serviços da AWS

Muitos serviços da AWS usam o KMS para fornecer criptografia em repouso:
*   **Amazon S3:** Criptografa objetos no lado do servidor (SSE-KMS).
*   **Amazon EBS:** Criptografa volumes de armazenamento.
*   **Amazon RDS:** Criptografa bancos de dados e snapshots.
*   **Amazon Redshift:** Criptografa clusters de data warehouse.

### Benefícios do KMS

*   **Totalmente Gerenciado:** Elimina a necessidade de operar sua própria infraestrutura de gerenciamento de chaves.
*   **Controle Centralizado:** Fornece um único ponto de controle para criar, gerenciar e usar chaves de criptografia em toda a AWS.
*   **Seguro e Conforme:** Usa HSMs validados pelo FIPS 140-2 e se integra ao AWS CloudTrail para fornecer logs de todo o uso de chaves para fins de auditoria e conformidade.
*   **Integração Profunda:** Integrado nativamente com dezenas de serviços da AWS, facilitando a criptografia de dados.
