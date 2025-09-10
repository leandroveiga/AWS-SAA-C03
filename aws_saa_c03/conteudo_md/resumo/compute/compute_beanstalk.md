### O que é AWS Elastic Beanstalk

O AWS Elastic Beanstalk é um serviço de orquestração que facilita a implantação e o escalonamento de aplicações e serviços da web desenvolvidos em linguagens como Java, .NET, PHP, Node.js, Python, Ruby, Go e Docker. Ele funciona como uma camada de automação sobre os serviços da AWS, como Amazon EC2, Amazon S3, Amazon RDS e Elastic Load Balancing.

### Como funciona o Elastic Beanstalk

1.  **Upload do Código:** Você simplesmente faz o upload do seu código-fonte (por exemplo, um arquivo .zip ou .war).
2.  **Configuração do Ambiente:** Você seleciona a plataforma (ex: Node.js, Python 3.8) e o Elastic Beanstalk provisiona e configura automaticamente a infraestrutura necessária para executar sua aplicação.
3.  **Provisionamento Automático:** O Beanstalk cuida de todos os detalhes, incluindo:
    *   Provisionamento de capacidade (instâncias EC2).
    *   Balanceamento de carga (Elastic Load Balancing).
    *   Escalonamento automático (Auto Scaling).
    *   Monitoramento da saúde da aplicação.
    *   Configuração de um proxy reverso (como Nginx ou Apache).
4.  **Gerenciamento e Monitoramento:** Após a implantação, o Elastic Beanstalk gerencia o ambiente. Ele aplica atualizações de plataforma, monitora a saúde das instâncias e fornece um painel centralizado para visualizar métricas e logs. Você pode se concentrar em escrever código em vez de gerenciar a infraestrutura.

### Componentes Principais

*   **Aplicação (Application):** Uma coleção lógica de ambientes, versões e configurações.
*   **Versão da Aplicação (Application Version):** Refere-se a uma iteração específica e rotulada do seu código.
*   **Ambiente (Environment):** Uma instância da sua aplicação em execução. Cada ambiente executa uma única versão da aplicação, mas você pode ter vários ambientes (ex: `dev`, `staging`, `prod`).
*   **Configuração do Ambiente (Environment Configuration):** Define os recursos e as políticas de comportamento do seu ambiente, como o tipo de instância EC2, configurações do load balancer e opções de banco de dados.

### Benefícios do Elastic Beanstalk

*   **Simplicidade e Produtividade:** Abstrai a complexidade da configuração da infraestrutura, permitindo que os desenvolvedores implantem aplicações rapidamente.
*   **Automação Completa:** Automatiza o provisionamento, o balanceamento de carga, o escalonamento e o monitoramento.
*   **Controle Total (Opcional):** Embora automatize tudo, ele não tira o controle. Você ainda pode acessar os recursos subjacentes (como as instâncias EC2) para personalizações avançadas.
*   **Custo:** Não há custo adicional pelo Elastic Beanstalk. Você paga apenas pelos recursos da AWS (EC2, S3, etc.) que sua aplicação consome.
