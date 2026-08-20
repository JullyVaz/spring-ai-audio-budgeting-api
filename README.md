# Spring AI - API Inteligente de Controle Financeiro

API desenvolvida em **Java com Spring Boot e Spring AI** para registro de transações financeiras por meio de comandos de voz.

O projeto foi desenvolvido como parte do desafio final do módulo de **Spring AI do Bootcamp Santander 1º Semestre de 2026**, utilizando arquitetura em camadas e integração com modelos da OpenAI.

---

## Objetivo

O objetivo do projeto é permitir que o usuário registre uma transação financeira utilizando linguagem natural por meio de um arquivo de áudio.

Por exemplo, o usuário pode enviar um áudio dizendo:

> "Gastei 50 reais no mercado."

A aplicação realiza automaticamente o seguinte processamento:

**Áudio → Transcrição → IA → Tool Calling → Caso de Uso → Persistência → Resposta em áudio**

---

## Fluxo da aplicação

```text
Cliente
  │
  │ Arquivo de áudio
  ▼
Spring Boot API
POST /transactions/ai
  │
  ▼
Speech-to-Text
OpenAI Whisper
  │
  ▼
ChatClient
Spring AI
  │
  ▼
Tool Calling
Casos de uso da aplicação
  │
  ▼
Persistência / Consulta
de transações
  │
  ▼
Text-to-Speech
OpenAI
  │
  ▼
Áudio MP3

Tecnologias utilizadas
Java 25
Spring Boot 4
Spring AI 2.0.0-M4
Spring Web
Spring Data JPA
MySQL
OpenAI API
Whisper - Speech-to-Text
OpenAI Text-to-Speech
Gradle
Docker Compose
REST API
Arquitetura

O projeto mantém uma separação entre domínio, aplicação e infraestrutura.

src/main/java/dio/budgeting
│
├── domain
│   ├── model
│   └── repository
│
├── application
│   ├── PersistTransactionUseCase
│   └── ListTransactionsByCategoryUseCase
│
└── infrastructure
    ├── http
    ├── persistence
    └── ...
Domain

Contém as entidades, regras e abstrações relacionadas ao domínio financeiro.

Application

Contém os casos de uso da aplicação.

Os casos de uso podem ser utilizados tanto pelos endpoints REST tradicionais quanto pelo mecanismo de Tool Calling do Spring AI.

Infrastructure

Responsável pelas integrações externas e adaptadores, incluindo:

API HTTP;
persistência;
integração com OpenAI;
processamento de áudio.
Integração com Spring AI
1. Speech-to-Text

O áudio enviado pelo usuário é processado utilizando:

TranscriptionModel

A aplicação utiliza o modelo Whisper para transformar o áudio em texto.

Exemplo:

Áudio:
"Gastei 50 reais no mercado."


        ↓


Transcrição:
"Gastei R$ 50,00 no mercado."
2. ChatClient

Após a transcrição, o texto é enviado ao ChatClient do Spring AI:

var result = chatClient
        .prompt()
        .user(userMessage)
        .call()
        .content();

O modelo interpreta a solicitação e identifica qual ação deve ser realizada.

3. Tool Calling

Os casos de uso da aplicação são registrados como ferramentas disponíveis para o modelo:

.defaultTools(
    persistTransactionUseCase,
    listTransactionsByCategoryUseCase
)

Dessa forma, a IA pode utilizar funcionalidades reais da aplicação em vez de apenas gerar uma resposta textual.

Por exemplo, ao receber:

"Gastei 50 reais no mercado."

a IA pode identificar a intenção de registrar uma nova transação e utilizar o caso de uso responsável pela persistência.

4. Text-to-Speech

Após o processamento da solicitação, a resposta textual da IA é convertida novamente para áudio:

byte[] audio = textToSpeechModel.call(result);

O endpoint retorna o resultado no formato:

audio/mp3
Endpoint de IA
POST
/transactions/ai
Content-Type
multipart/form-data
Parâmetro
file

Exemplo utilizando curl:

curl.exe -X POST `
  -F "file=@gasto.mp3.m4a" `
  http://localhost:8080/transactions/ai `
  -o resposta.mp3

A resposta é um arquivo de áudio MP3.

Endpoints de transações
Criar transação
POST /transactions
Listar transações por categoria
GET /transactions/{category}
Tratamento de requisições inválidas

Durante o desenvolvimento, também foram realizados testes de validação do endpoint.

HTTP 400 - Bad Request

O endpoint rejeita uma requisição inválida quando os dados enviados não podem ser processados corretamente.

HTTP 415 - Unsupported Media Type

O endpoint também valida o tipo de conteúdo recebido.

Durante os testes foi possível obter:

HTTP/1.1 415

Esse comportamento demonstra a validação do formato de requisição esperado pelo endpoint.

Evidências dos testes

As evidências dos testes estão disponíveis no diretório:

docs/evidencias/
1. Teste com áudio válido

Demonstra o envio do arquivo de áudio para o endpoint.

2. Resposta de áudio gerada

Demonstra a geração do arquivo resposta.mp3.

3. Resposta HTTP 200

Demonstra o processamento bem-sucedido da requisição e o retorno do áudio:

HTTP/1.1 200
Content-Type: audio/mp3

4. Validação HTTP 415

Demonstra a rejeição de uma requisição enviada com tipo de conteúdo incompatível.

Configuração

A aplicação utiliza uma variável de ambiente para armazenar a chave da API da OpenAI.

Windows PowerShell
$env:OPENAI_API_KEY="sua_chave_aqui"
Linux / macOS
export OPENAI_API_KEY="sua_chave_aqui"

A chave da API não deve ser armazenada diretamente no código ou enviada ao GitHub.

Executando o projeto

Clone o repositório:

git clone <URL_DO_REPOSITORIO>

Entre no diretório:

cd spring-ai-audio-budgeting-api

Configure a variável de ambiente:

OPENAI_API_KEY

Inicie a aplicação.

Linux / macOS
./gradlew bootRun
Windows
.\gradlew.bat bootRun
Testando a API

Com a aplicação em execução:

curl.exe -X POST `
  -F "file=@gasto.mp3.m4a" `
  http://localhost:8080/transactions/ai `
  -o resposta.mp3

Após o processamento, o arquivo:

resposta.mp3

será gerado localmente.

Os arquivos de áudio utilizados durante os testes são ignorados pelo .gitignore e não são enviados ao repositório.

Principais conceitos de Spring AI utilizados
TranscriptionModel
ChatClient
Tool Calling
TextToSpeechModel
integração com OpenAI
processamento de áudio
integração de IA com casos de uso da aplicação
Documentação
Spring AI
ChatClient
Tools
Audio Transcriptions
Audio Speech
Aprendizados

Este projeto permitiu aplicar conceitos de:

desenvolvimento de APIs REST;
arquitetura em camadas;
separação entre domínio, aplicação e infraestrutura;
integração de Inteligência Artificial em uma aplicação backend;
Speech-to-Text;
processamento de linguagem natural;
Tool Calling;
Text-to-Speech;
tratamento de erros HTTP;
integração com serviços externos;
configuração segura de credenciais;
testes de API utilizando curl.