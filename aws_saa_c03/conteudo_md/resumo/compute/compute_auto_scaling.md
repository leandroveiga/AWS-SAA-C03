### O que é Amazon EC2 Auto Scaling

O Amazon EC2 Auto Scaling é um serviço da Amazon Web Services (AWS) que ajusta automaticamente o número de instâncias do Amazon EC2 em seu ambiente na nuvem para corresponder à demanda. Ele monitora a utilização dos recursos, como a carga da CPU, e adiciona ou remove instâncias EC2 conforme necessário para manter o desempenho consistente e otimizar custos.

### Como funciona o Amazon EC2 Auto Scaling

1.  **Monitora a Demanda:** O serviço monitora continuamente métricas de desempenho de suas instâncias EC2 e do tráfego, como a utilização da CPU, e verifica a saúde das instâncias.
2.  **Aplica as Políticas:** Com base nas políticas de escalabilidade definidas (por exemplo, "adicionar uma instância quando a utilização da CPU exceder X%"), o EC2 Auto Scaling decide se é necessário escalar.
3.  **Ajusta a Capacidade:**
    *   **Para escalar para cima (Scale-out):** Adiciona novas instâncias do EC2 para lidar com o aumento da demanda, garantindo que os aplicativos mantenham o desempenho.
    *   **Para escalar para baixo (Scale-in):** Remove instâncias quando a demanda diminui, o que reduz custos, pois você só paga pelos recursos que utiliza.
4.  **Gerencia a Integridade:** Se uma instância se tornar prejudicada ou deixar de responder, o EC2 Auto Scaling a substitui automaticamente por uma nova instância saudável.

### Componentes Principais

*   **Grupos de Auto Scaling (Auto Scaling Groups):** Um conjunto de instâncias do EC2 que são tratadas como uma unidade lógica para escalabilidade e gerenciamento. Você define o número mínimo, máximo e desejado de instâncias.
*   **Modelos de Inicialização (Launch Templates):** Definem os parâmetros para as novas instâncias do EC2 a serem lançadas (como o tipo de instância, Amazon Machine Image (AMI), security groups, etc.).
*   **Políticas de Escalonamento (Scaling Policies):** Regras que especificam quando e como adicionar ou remover instâncias (ex: Target Tracking, Scheduled Scaling).

### Benefícios do EC2 Auto Scaling

*   **Disponibilidade e Resiliência:** Mantém a alta qualidade de serviço e o desempenho dos aplicativos, substituindo instâncias com falha automaticamente.
*   **Custo-Benefício:** Garante que você não pague por capacidade de recursos que não são necessárias, otimizando os custos.
*   **Elasticidade:** Permite automatizar o processo de escalonamento sem intervenção manual constante, adaptando-se dinamicamente à demanda.
