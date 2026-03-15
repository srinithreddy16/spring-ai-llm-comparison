# Spring AI — LLM Comparison API

A Spring Boot REST API that exposes multiple large language model (LLM) providers behind a single set of endpoints, so you can compare responses from **OpenAI**, **Anthropic (Claude)**, and **Ollama** from one place.

## Features

- **Multi-provider support** — OpenAI, Anthropic, and Ollama behind unified REST endpoints
- **Spring AI 2.x** — Built with [Spring AI](https://docs.spring.io/spring-ai/reference/) 2.0.0-M2
- **CORS enabled** — Ready to be called from a browser or a separate UI
- **Configurable models** — Set API keys and model names via properties or environment variables

## Tech Stack

| Component   | Version / Technology |
|------------|----------------------|
| Java       | 21                   |
| Spring Boot| 4.0.3                |
| Spring AI  | 2.0.0-M2             |
| Build      | Maven                |

## Prerequisites

- **JDK 21**
- **Maven 3.6+**
- For **OpenAI** and **Anthropic**: API keys from [OpenAI](https://platform.openai.com/api-keys) and [Anthropic](https://console.anthropic.com/settings/keys)
- For **Ollama**: [Ollama](https://ollama.com/) installed and running locally, with at least one model pulled (e.g. `ollama pull deepseek-r1:7b`)

## Quick Start

### 1. Clone and build

```bash
git clone https://github.com/srinithreddy16/spring-ai-llm-comparison.git
cd spring-ai-llm-comparison
./mvnw spring-boot:run
```

The API runs at **http://localhost:8080**.

### 2. Configure API keys (optional per provider)

Create `src/main/resources/application-local.properties` (this file is gitignored):

```properties
# Only set the keys for the providers you want to use
spring.ai.openai.api-key=<your-openai-api-key>
spring.ai.anthropic.api-key=<your-anthropic-api-key>
```

Or use environment variables:

- `SPRING_AI_OPENAI_API_KEY`
- `SPRING_AI_ANTHROPIC_API_KEY`

Ollama does not require an API key; ensure the chosen model is pulled (e.g. `ollama pull deepseek-r1:7b`).

### 3. Model configuration

In `application.properties` (or override in `application-local.properties`):

```properties
spring.ai.anthropic.chat.options.model=claude-sonnet-4-6
spring.ai.ollama.chat.options.model=deepseek-r1:7b
```

Adjust model names as needed for your setup.

## API Endpoints

All endpoints accept a **GET** request with the user message in the path or as a query parameter. Responses are plain text (the model’s reply).

| Provider   | Endpoint                    | Example                                      |
|-----------|-----------------------------|----------------------------------------------|
| OpenAI    | `GET /api/openai/{message}` | `http://localhost:8080/api/openai/Hello`    |
| Anthropic | `GET /api/anthropic/{message}` | `http://localhost:8080/api/anthropic/Hello` |
| Ollama    | `GET /api/ollama/{message}` | `http://localhost:8080/api/ollama/Hello`     |

**Note:** For messages with spaces or special characters, consider encoding the path or adding a query-parameter variant (e.g. `?message=...`) in your client.

## Testing with the UI

A React + Vite frontend is available for testing this API:

- **Repository:** [llm-comparison-ui](https://github.com/srinithreddy16/llm-comparison-ui)
- **Clone and run:**
  ```bash
  git clone https://github.com/srinithreddy16/llm-comparison-ui.git
  cd llm-comparison-ui
  npm install
  npm run dev
  ```
- Point the UI’s API base URL to `http://localhost:8080` (or your deployed backend) so it calls the endpoints above. Use it only in a development/testing setting.

## Project structure

```
src/main/java/org/srinith/springai/
├── SpringAiApplication.java   # Entry point
├── OpenAIController.java      # /api/openai
├── AnthropicController.java  # /api/anthropic
└── OllamaController.java     # /api/ollama
```

## Security

- **Do not commit API keys.** Use `application-local.properties` (gitignored) or environment variables.
- Rotate any key that may have been committed or exposed.

## License

See the repository for license information.
