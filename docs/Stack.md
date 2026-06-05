## Backend

| Camada                  | Tecnologia                                             |
| ----------------------- | ------------------------------------------------------ |
| **Linguagem**           | Python 3.11+                                           |
| **Framework Web**       | FastAPI                                                |
| **Servidor ASGI**       | Uvicorn                                                |
| **ORM**                 | SQLAlchemy 2.x                                         |
| **Validação**           | Pydantic v2 + Pydantic Settings                        |
| **Banco de Dados**      | SQLite (padrão), PostgreSQL (via `DATABASE_URL`)       |
| **Vector Store**        | ChromaDB                                               |
| **Embeddings**          | FastEmbed (ONNX local) / HTTP API de embeddings        |
| **Autenticação**        | bcrypt + TOTP (2FA) + tokens de sessão                 |
| **Criptografia**        | Fernet (symmetric, via `cryptography`)                 |
| **Busca Web**           | SearXNG (auto-hospedado, Docker)                       |
| **Provedores de Busca** | SearXNG, Brave, DuckDuckGo, Google PSE, Tavily, Serper |
| **Notificações**        | ntfy (Docker)                                          |
| **Agendamento**         | asyncio + croniter (cron-style scheduler)              |
| **HTTP Client**         | httpx                                                  |
| **MCP**                 | Model Context Protocol (`mcp` package)                 |
| **Testes**              | pytest + pytest-asyncio                                |
| **Geração de Imagem**   | Diffusers (via `diffusion_server.py`)                  |
| **STT Local**           | faster-whisper (opcional)                              |
| **CLI**                 | Scripts shell em `scripts/`                            |

## Frontend

| Camada | Tecnologia |
|---|---|
| **Arquitetura** | Vanilla JavaScript ES Modules (sem framework) |
| **Estilização** | CSS puro (sem pré-processador) |
| **Fontes** | Fira Code (mono), Inter (sans) |
| **Markdown** | Custom renderer próprio |
| **PWA** | Service Worker + Manifest |
| **Syntax Highlight** | highlight.js |
| **Build** | Nenhum — módulos ES servidos diretamente pelo FastAPI |
| **Edição de Imagem** | Canvas API nativa |
| **Reconhecimento Facial** | face_recognition (via `services/faces/`) |


## Infraestrutura


| Camada | Tecnologia |
|---|---|
| **Containerização** | Docker + Docker Compose |
| **GPU NVIDIA** | CUDA (docker-compose overlay) |
| **GPU AMD** | ROCm (docker-compose overlay) |
| **GPU Apple** | Metal (via `start-macos.sh`) |
| **CI/CD** | GitHub Actions |
| **Entrypoint Docker** | gosu (privilege dropping) |


## Dependências Externas (Serviços Docker)

| Serviço | Função |
|---|---|
| **chromadb/chroma** | Vector store para memória e RAG |
| **searxng/searxng** | Busca web auto-hospedada |
| **binwiederhier/ntfy** | Notificações push |


## Integrações

| Integração                                  | Tipo                        |
| ------------------------------------------- | --------------------------- |
| **OpenAI API**                              | Provedor de LLM             |
| **Anthropic API**                           | Provedor de LLM             |
| **GitHub Copilot**                          | Chat via OAuth device-flow  |
| **OpenRouter**                              | Roteador multi-modelo       |
| **vLLM / llama.cpp / Ollama**               | Servidores locais           |
| **Qualquer endpoint compatível com OpenAI** | Provedor genérico           |
| **Claude Code**                             | Skill bundle para terminal  |
| **Codex CLI**                               | Plugin de integração        |
| **CalDAV**                                  | Sincronização de calendário |
| **CardDAV**                                 | Sincronização de contatos   |
| **IMAP/SMTP**                               | Gerenciamento de email      |

  

## DevOps / Ferramentas

  

| Ferramenta | Uso |
|---|---|
| **ruff** | Linter Python |
| **mypy** | Type checker Python (opcional) |
| **compileall** | Validação de sintaxe Python (CI) |
| **Node.js** | Validação de sintaxe JS (CI) |
