 

## Visão Geral

  

**Odysseus** é uma aplicação monolítica em camadas (layered monolith) com backend FastAPI e frontend SPA vanilla JavaScript. O projeto é auto-hospedado (self-hosted), privado por design, e funciona como um workspace de IA completo.

  

```

┌──────────────────────────────────────────────────────────────┐

│                      Cliente (Browser)                        │

│              Vanilla JS SPA (ES Modules)                      │

│         static/index.html + static/app.js + static/js/*        │

└─────────────────────────┬────────────────────────────────────┘

                          │ HTTP / SSE

                          ▼

┌──────────────────────────────────────────────────────────────┐

│                    FastAPI (app.py)                           │

│                                                              │

│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐  │

│  │  Auth &  │  │  Routes  │  │   src/   │  │  Services  │  │

│  │ Security │──│ (50 rotas)│──│(lógica de │──│ (dominio)  │  │

│  │(core/auth)│  │ routes/* │  │  negócio) │  │ services/* │  │

│  └──────────┘  └──────────┘  └──────────┘  └────────────┘  │

│                                                              │

│  ┌──────────┐  ┌──────────┐  ┌─────────────────────────┐    │

│  │  MCP     │  │  Tasks   │  │  CLI (scripts/*)        │    │

│  │  Servers │  │  Scheduler│  │  odysseus-* commands    │    │

│  └──────────┘  └──────────┘  └─────────────────────────┘    │

└──────────────────────────────────────────────────────────────┘

```

  

## Estrutura de Diretórios

  

```

odysseus/

├── app.py                 # Entry point: montagem do FastAPI, middlewares, rotas

├── core/                  # Infraestrutura central

│   ├── auth.py            # AuthManager: usuários, sessões, 2FA

│   ├── database.py        # Modelos SQLAlchemy (~25 tabelas)

│   ├── middleware.py       # Security headers, require_admin

│   ├── session_manager.py # Gerenciamento de sessões de chat

│   └── ...

├── src/                   # Lógica de negócio (84 arquivos)

│   ├── agent_loop.py      # Loop principal do agente

│   ├── llm_core.py        # Chamadas LLM (streaming + sync)

│   ├── tool_implementations.py  # 50+ ferramentas do agente

│   ├── deep_research.py   # Motor de pesquisa profunda

│   ├── memory.py          # Gerenciamento de memória persistente

│   ├── rag_manager.py     # RAG (Retrieval-Augmented Generation)

│   ├── config.py          # Configurações Pydantic

│   ├── chat_handler.py    # Handler de chat

│   ├── search/            # Módulo de busca web (orquestração)

│   └── ...

├── routes/                # Handlers de API (50 arquivos)

│   ├── chat_routes.py     # Chat streaming + contexto

│   ├── auth_routes.py     # Login/logout/signup/2FA

│   ├── email_routes.py    # Email CRUD + envio

│   ├── calendar_routes.py # Calendário CalDAV

│   ├── research_routes.py # Pesquisa profunda

│   └── ...

├── services/              # Serviços de domínio encapsulados

│   ├── hwfit/             # Hardware fit scoring (GPU/VRAM)

│   ├── memory/            # Serviço de memória

│   ├── research/          # Serviço de pesquisa

│   ├── search/            # Serviço de busca (provedores)

│   ├── docs/              # Serviço de documentos

│   ├── stt/               # Speech-to-text

│   ├── tts/               # Text-to-speech

│   └── youtube/           # YouTube transcripts

├── static/                # Frontend SPA

│   ├── index.html         # SPA principal (2298 linhas)

│   ├── app.js             # Orquestrador JS (4080 linhas)

│   ├── style.css          # CSS consolidado (35922 linhas)

│   ├── js/                # Módulos ES (78 arquivos)

│   │   ├── chat.js        # UI do chat

│   │   ├── chatStream.js  # Streaming SSE

│   │   ├── document.js    # Editor de documentos

│   │   ├── emailInbox.js  # Caixa de email

│   │   ├── calendar/      # Sub-módulos de calendário

│   │   ├── settings.js    # UI de configurações

│   │   ├── theme.js       # Gerenciamento de tema

│   │   └── ...

│   └── lib/               # Bibliotecas vendor

├── mcp_servers/            # Servidores MCP built-in

│   ├── email_server.py     # Ferramentas de email via MCP

│   ├── memory_server.py    # Gerenciamento de memória via MCP

│   ├── rag_server.py       # RAG via MCP

│   └── image_gen_server.py # Geração de imagem via MCP

├── tests/                  # Suite de testes (404 arquivos)

├── scripts/                # CLI tools (36 comandos)

├── companion/              # Bridge para clientes LAN

├── integrations/           # Integrações externas (Claude, Codex)

├── docker/                 # Docker entrypoint + GPU overlays

├── docs/                   # Documentação e demos

└── data/                   # Dados runtime (gitignored)

    ├── app.db              # SQLite database

    ├── auth.json           # Contas de usuário

    ├── settings.json       # Configurações

    └── ...

```

  

## Fluxo de Requisição

  

```

HTTP Request

  → FastAPI app

    → CORS Middleware

      → SecurityHeadersMiddleware (CSP, HSTS, X-Frame-Options)

        → RequestTimeoutMiddleware

          → AuthMiddleware (cookie session ou bearer token)

            → Route Handler (routes/*.py)

              → src/ business logic

                → Database (SQLAlchemy) / Services

                  → Response (JSON ou SSE streaming)

```

  

## Fluxo do Chat com Agente

  

```

User Message

  → ChatHandler

    → Context Builder (memória + personal docs + RAG)

      → LLM Call (stream_llm)

        → [Agent Mode] stream_agent_loop

          → Parse tool blocks da resposta do LLM

            → Executar ferramentas (tool_execution)

              → Formatar resultados

                → Feedback ao LLM (até 20 rounds)

                  → Response streaming (SSE)

```

  

## Fluxo de Memória

  

```

Chat History

  → MemoryExtractor

    → Categorização + extração de texto

      → MemoryManager (JSON + SQLite)

        → MemoryVectorStore (ChromaDB + FastEmbed)

          → Retrieval híbrido (keyword + vetorial + BM25)

```

  

## Frontend SPA

  

- **Orquestrador:** `app.js` importa todos os módulos e os interliga

- **Estado:** `storage.js` abstrai localStorage

- **UI:** `ui.js` fornece utilitários DOM, modais, sistema de eventos

- **Roteamento:** Navegação SPA baseada em hash (`#section`)

- **API:** `window.fetch` nativo com interceptação de 401

- **Templating:** Manipulação direta do DOM via `innerHTML`

- **Streaming:** `chatStream.js` consome SSE (Server-Sent Events)

- **Tema:** `theme.js` gerencia temas claro/escuro com CSS custom properties

  

## Padrões de Código

  

- **Rotas:** Funções `setup_<nome>_routes() → APIRouter` montadas em `/api/`

- **Autenticação:** Cookie de sessão ou token Bearer `ody_`; `request.state.current_user`

- **Escopo:** Filtro `owner_filter` para isolar dados por usuário

- **Admin:** Rotas administrativas protegidas por `require_admin`

- **Config:** Pydantic Settings com suporte a `.env`

- **Testes:** pytest com pytest-asyncio; stubs de dependências pesadas via MagicMock

  

## Banco de Dados (SQLAlchemy)

  

Tabelas principais em `core/database.py`:

- `Session` / `ChatMessage` — Sessões e mensagens de chat

- `Document` / `DocumentVersion` — Documentos com versionamento

- `GalleryImage` / `GalleryAlbum` — Galeria de imagens com metadados EXIF

- `EmailAccount` — Contas IMAP/SMTP (senhas criptografadas)

- `ModelEndpoint` — Configurações de endpoints LLM

- `McpServer` — Configurações de servidores MCP

- `ApiToken` — Tokens de API para integrações

- `Webhook` — Webhooks de saída

- `ScheduledTask` / `TaskRun` — Tarefas agendadas

- `Memory` — Entradas de memória persistente