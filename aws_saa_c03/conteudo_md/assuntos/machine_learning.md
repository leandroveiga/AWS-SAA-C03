# Machine Learning na AWS

**Serviços Cobertos:** Comprehend, Rekognition, Transcribe, SageMaker.

A AWS oferece um conjunto de serviços de Inteligência Artificial (IA) e Machine Learning (ML) que permitem aos desenvolvedores adicionar inteligência a aplicações sem a necessidade de ter um conhecimento profundo em ML. Esses serviços podem ser divididos em duas categorias principais: serviços de IA de alto nível (modelos pré-treinados) e a plataforma SageMaker (para construção de modelos personalizados).

---

## 1. Amazon Comprehend

O Amazon Comprehend é um serviço de processamento de linguagem natural (NLP) que usa machine learning para encontrar insights e relações em textos. Não é necessário ter experiência em ML para usá-lo.

-   **Principais Funcionalidades:**
    -   **Análise de Sentimento:** Determina o sentimento de um texto (Positivo, Negativo, Neutro ou Misto). Útil para analisar feedback de clientes, menções em redes sociais, etc.
    -   **Reconhecimento de Entidades:** Identifica entidades nomeadas em um texto, como pessoas, lugares, marcas e datas.
    -   **Extração de Frases-Chave:** Extrai as frases mais importantes de um texto.
    -   **Detecção de Idioma:** Identifica o idioma principal de um texto.
    -   **Modelos Personalizados:** Você pode treinar o Comprehend com seus próprios dados para criar modelos de classificação e reconhecimento de entidades específicos para o seu domínio de negócio.

-   **Casos de Uso:**
    -   Analisar o feedback dos clientes em e-mails e formulários para entender a satisfação.
    -   Processar artigos de notícias para extrair informações sobre empresas e produtos.
    -   Moderar comentários em um fórum para identificar conteúdo impróprio.

---

## 2. Amazon Rekognition

O Amazon Rekognition é um serviço que facilita a adição de análise de imagem e vídeo às suas aplicações. Ele usa tecnologia de deep learning para analisar imagens e vídeos, identificando objetos, pessoas, texto, cenas e atividades.

-   **Principais Funcionalidades:**
    -   **Detecção de Objetos e Cenas:** Identifica milhares de objetos (como "carro", "bicicleta") e cenas (como "praia", "pôr do sol").
    -   **Análise Facial:** Detecta rostos em imagens e vídeos, analisa atributos faciais (como "sorrindo", "usa óculos") e compara rostos.
    -   **Reconhecimento de Celebridades:** Reconhece centenas de milhares de celebridades.
    -   **Moderação de Conteúdo:** Detecta conteúdo explícito ou impróprio em imagens e vídeos.
    -   **Detecção de Texto:** Extrai texto de imagens, como placas de rua, números em fotos, etc.
    -   **Análise de Vídeo:** Pode rastrear o movimento de pessoas e analisar atividades em vídeos armazenados ou em streaming.

-   **Casos de Uso:**
    -   Indexar uma biblioteca de fotos para torná-la pesquisável por conteúdo.
    -   Verificar a identidade de usuários comparando uma selfie com uma foto de documento.
    -   Moderar imagens carregadas por usuários em uma rede social.
    -   Analisar vídeos de segurança para detectar atividades específicas.

---

## 3. Amazon Transcribe

O Amazon Transcribe é um serviço de reconhecimento automático de fala (ASR) que facilita a conversão de áudio em texto. Ele usa modelos de machine learning para criar transcrições precisas de arquivos de áudio e vídeo.

-   **Principais Funcionalidades:**
    -   **Transcrição em Lote e em Tempo Real:** Pode transcrever arquivos de áudio armazenados ou streams de áudio em tempo real.
    -   **Identificação de Idioma:** Pode detectar automaticamente o idioma falado no áudio.
    -   **Identificação de Múltiplos Oradores (Speaker Diarization):** Consegue identificar quem falou o quê em um áudio com várias pessoas.
    -   **Vocabulário Personalizado:** Permite que você adicione palavras específicas do seu domínio (como nomes de produtos ou jargões técnicos) para melhorar a precisão da transcrição.
    -   **Redação de PII:** Pode identificar e redigir (remover) informações de identificação pessoal das transcrições.

-   **Casos de Uso:**
    -   Transcrever chamadas de atendimento ao cliente para análise de sentimento e conformidade.
    -   Gerar legendas para conteúdo de vídeo.
    -   Converter reuniões gravadas ou palestras em texto pesquisável.
    -   Criar assistentes de voz e comandos de voz em aplicações.

---

## 4. Amazon SageMaker

O Amazon SageMaker é uma plataforma totalmente gerenciada que permite a desenvolvedores e cientistas de dados construir, treinar e implantar modelos de machine learning (ML) em escala. Ele abrange todo o ciclo de vida do ML.

-   **Diferença para outros serviços de IA:** Enquanto serviços como Rekognition, Comprehend e Transcribe fornecem modelos pré-treinados para tarefas específicas, o SageMaker é uma plataforma para **construir, treinar e hospedar seus próprios modelos personalizados**.

-   **Principais Componentes do Ciclo de Vida do ML:**
    -   **Preparação (Build):** Inclui o **SageMaker Studio**, um IDE completo para ML, e instâncias de notebook Jupyter gerenciadas para explorar e preparar dados.
    -   **Treinamento (Train):** Simplifica o processo de treinamento de modelos. Você pode usar algoritmos integrados, trazer seus próprios algoritmos ou usar frameworks de ML populares (como TensorFlow, PyTorch). O SageMaker gerencia a infraestrutura de treinamento, incluindo a otimização de custos com instâncias Spot.
    -   **Implantação (Deploy):** Após o treinamento, o SageMaker facilita a implantação do modelo em um endpoint HTTPS para obter previsões (inferência) em tempo real ou em lote. Ele gerencia o auto scaling para lidar com a carga de trabalho.

-   **Casos de Uso:**
    -   Criar um modelo de recomendação de produtos para um site de e-commerce.
    -   Desenvolver um modelo de previsão de churn (cancelamento) de clientes.
    -   Construir um sistema de detecção de fraude financeira personalizado.
    -   Análise preditiva para otimização de cadeia de suprimentos.
