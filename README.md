# Spring AI - API Inteligente de Controle Financeiro

API desenvolvida em Java, Spring Boot e Spring AI para registro de transações financeiras por meio de comandos de voz.

Projeto desenvolvido como parte do desafio final do módulo de Spring AI do Bootcamp Santander 1º Semestre de 2026.

---

## Objetivo

Permitir o registro de transações financeiras utilizando comandos de voz.

O usuário envia um áudio, que é transcrito e interpretado pela Inteligência Artificial. A aplicação utiliza Tool Calling para acionar os casos de uso responsáveis pelo processamento da transação e, ao final, gera uma resposta em áudio.

Exemplo:

"Gastei R$ 50,00 no mercado."

Fluxo principal:

Áudio → Speech-to-Text → ChatClient → Tool Calling → Caso de Uso → Persistência → Text-to-Speech → Áudio MP3

---

## Recursos de Inteligência Artificial

- Speech-to-Text: conversão do áudio em texto utilizando TranscriptionModel.
- ChatClient: processamento da solicitação utilizando Spring AI.
- Tool Calling: acionamento dos casos de uso da aplicação a partir da interpretação da IA.
- Text-to-Speech: conversão da resposta final em áudio utilizando TextToSpeechModel.

---

## Arquitetura

O projeto utiliza arquitetura em camadas, mantendo a separação entre domínio, aplicação e infraestrutura.

src/main/java/dio/budgeting

├── domain
├── application
└── infrastructure

Os casos de uso são reutilizados tanto pelos endpoints REST quanto pelo mecanismo de Tool Calling, mantendo as regras de negócio independentes da integração com a IA.

---

## Melhoria implementada

Além da implementação do fluxo de Speech-to-Text, Tool Calling e Text-to-Speech, foi implementada uma melhoria no endpoint /transactions/ai para aumentar a robustez da API.

### Validação do tipo de mídia

O endpoint passou a validar o tipo de conteúdo recebido antes de realizar o processamento do áudio.

Quando é enviada uma requisição com formato de mídia não suportado, a API retorna:

HTTP 415 - Unsupported Media Type

Essa melhoria contribui para:

- maior robustez da API;
- validação das entradas;
- redução de chamadas inválidas ao serviço de IA;
- tratamento mais adequado de erros HTTP.

A validação foi testada e registrada nas evidências do projeto.

---

## Evidências

Foram realizados testes do fluxo de processamento de áudio e da validação de requisições inválidas.

As evidências estão disponíveis no diretório:

docs/evidencias/

Arquivos:

- 01-teste-audio-valido.jpg — envio e processamento do áudio.
- 02-resposta-audio-gerada.jpg — geração da resposta em áudio.
- 03-resposta-http-200.jpg — processamento bem-sucedido com resposta HTTP 200.
- 04-teste-415.png — validação do tipo de mídia com retorno HTTP 415.

---

## Tecnologias

- Java
- Spring Boot
- Spring AI
- OpenAI API
- Gradle
- JPA / Hibernate
- MySQL
- Docker / Docker Compose
- REST API
- Git / GitHub

### Conceitos aplicados

- Arquitetura em camadas
- Domain-Driven Design (DDD)
- Clean Architecture
- Use Cases
- Repository Pattern
- Tool Calling
- Speech-to-Text
- Text-to-Speech
- Validação de entrada
- Tratamento de erros HTTP

---

## Endpoint principal

### Processamento de transação por áudio

POST /transactions/ai

Content-Type: multipart/form-data

Parâmetro: file

Exemplo utilizando curl:

curl -X POST -F "file=@gasto.mp3.m4a" http://localhost:8080/transactions/ai -o resposta.mp3

O endpoint recebe o áudio, realiza a transcrição, utiliza a Inteligência Artificial para interpretar a solicitação e retorna a resposta processada em formato de áudio.

O arquivo resposta.mp3 é gerado localmente após o processamento.

Os arquivos de áudio utilizados nos testes não fazem parte do repositório.

---

## Outros endpoints

### Criar transação

POST /transactions

### Listar transações por categoria

GET /transactions/{category}

---

## Como executar

### Pré-requisitos

- Java
- Docker Desktop
- Git
- Chave de API da OpenAI

### Windows PowerShell

Configure a variável de ambiente:

$env:OPENAI_API_KEY="sua_chave_aqui"

### Linux/macOS

Configure a variável de ambiente:

export OPENAI_API_KEY="sua_chave_aqui"

Nunca coloque sua chave da OpenAI diretamente no código ou no GitHub.

### Executar a aplicação - Windows

.\gradlew.bat bootRun

### Executar a aplicação - Linux/macOS

./gradlew bootRun

### Executar os testes - Windows

.\gradlew.bat test

### Executar os testes - Linux/macOS

./gradlew test

---

## Estrutura

spring-ai-audio-budgeting-api

├── docs
│   └── evidencias
│       ├── 01-teste-audio-valido.jpg
│       ├── 02-resposta-audio-gerada.jpg
│       ├── 03-resposta-http-200.jpg
│       └── 04-teste-415.png
│
├── gradle
│   └── wrapper
│
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
│   └── test
│
├── .gitignore
├── build.gradle
├── compose.yml
├── gradlew
├── gradlew.bat
├── README.md
├── requests.http
└── settings.gradle

---

## Documentação

- Spring AI Reference:
  https://docs.spring.io/spring-ai/reference/

- ChatClient:
  https://docs.spring.io/spring-ai/reference/api/chatclient.html

- Tools / Tool Calling:
  https://docs.spring.io/spring-ai/reference/api/tools.html

- Audio Transcriptions:
  https://docs.spring.io/spring-ai/reference/api/audio/transcriptions.html

- Audio Speech:
  https://docs.spring.io/spring-ai/reference/api/audio/speech.html

---

## Contexto

Projeto desenvolvido durante o Bootcamp Santander 1º Semestre de 2026, no desafio final do módulo de Spring AI.

O projeto teve como foco a integração de recursos de Inteligência Artificial em uma API backend, utilizando processamento de voz, Tool Calling e geração de áudio, além da implementação de validações para tornar o endpoint mais robusto.

---

## Autora

Juliane Vaz

Desenvolvedora Back-End | Java | C#/.NET | Python

GitHub:
https://github.com/JullyVaz

