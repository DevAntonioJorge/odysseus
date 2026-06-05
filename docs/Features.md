

## Chat & Agente

  

- **Chat multi-modelo** — Suporta vLLM, llama.cpp, Ollama, OpenAI, OpenRouter, GitHub Copilot e qualquer endpoint compatível com OpenAI

- **Modo agente** — LLM executa múltiplos rounds de ferramentas automaticamente (até 20 rounds)

- **Streaming SSE** — Respostas em tempo real via Server-Sent Events

- **Gerenciamento de contexto** — Budget tracking, compactação e truncamento automático

- **Presets** — System prompts, temperaturas, nomes de personagens configuráveis

- **Slash commands** — Ações rápidas via `/` no input

- **Injeção de memória** — Retrieval híbrido (keyword + vetorial) no contexto do chat

- **Group chat** — Sessões com múltiplos participantes

- **Comparação A/B** — Compara respostas de dois modelos lado a lado

  

## Ferramentas do Agente (50+)

  

O agente tem acesso a ferramentas organizadas como fenced code blocks ou OpenAI function calls:

  

| Categoria         | Ferramentas                                                                                                                                                                                                            |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Shell**         | bash, python                                                                                                                                                                                                           |
| **Arquivos**      | read, write, edit, grep, glob, ls                                                                                                                                                                                      |
| **Web**           | web_search, web_fetch                                                                                                                                                                                                  |
| **Documentos**    | create_document, update_document, edit_document, suggest_document                                                                                                                                                      |
| **Chat**          | search_chats, chat_with_model, create_session, list_sessions, send_to_session                                                                                                                                          |
| **Memória**       | manage_memory, manage_skills                                                                                                                                                                                           |
| **Tarefas/Notas** | manage_tasks, manage_notes                                                                                                                                                                                             |
| **Calendário**    | manage_calendar                                                                                                                                                                                                        |
| **Email**         | send_email, list_emails, read_email, reply_to_email, bulk_email, archive_email, delete_email, mark_email_read, list_email_accounts, resolve_contact, manage_contact                                                    |
| **Cookbook**      | download_model, serve_model, list_served_models, stop_served_model, list_downloads, cancel_download, search_hf_models, list_cached_models, list_serve_presets, serve_preset, adopt_served_model, list_cookbook_servers |
| **Imagens**       | generate_image, edit_image                                                                                                                                                                                             |
| **MCP**           | manage_mcp, manage_webhooks, manage_tokens                                                                                                                                                                             |
| **Pesquisa**      | trigger_research, manage_research                                                                                                                                                                                      |
| **Sistema**       | manage_session, list_models, manage_endpoints, manage_settings, api_call, ask_teacher, ui_control, app_api                                                                                                             |

  

## Cookbook (Gerenciamento de Modelos)

  

- **Recomendação hardware-aware** — Scoring de GPU (VRAM, formato de quantização) via `services/hwfit/`

- **Download de modelos** — Do HuggingFace com progresso tracking

- **Servidor local** — Via vLLM, llama.cpp ou Ollama

- **Servidor remoto** — Suporte a SSH para servidores remotos

- **Detecção de GPU Docker** — Passthrough NVIDIA e AMD

- **Gerenciamento de dependências** — Instalação automática de engines de serve

  

## Pesquisa Profunda (Deep Research)

  

- **Loop Think-Search-Extract-Synthesize** — Iterativo e auto-refinável

- **Planos multi-etapa** — Sub-questões geradas automaticamente

- **Coleta de fontes** — Extração de conteúdo de páginas web

- **Relatórios visuais** — Geração de HTML com renderização estruturada

- **Profundidade configurável** — Controle sobre escopo e iterações

  

## Sistema de Memória

  

- **Memória persistente** — Armazenada em JSON e SQLite simultaneamente

- **Memória vetorial** — Via ChromaDB com embeddings FastEmbed ONNX

- **Retrieval híbrido** — Keyword + vetorial + scoring BM25

- **Extração automática** — Do histórico de conversas por categoria

- **Skills** — Comportamentos reutilizáveis que evoluem com o uso

- **Extração de skills** — Criação automática a partir de interações do usuário

  

## Email

  

- **Multi-contas** — Suporte a múltiplas contas IMAP/SMTP

- **Triage por IA** — Detecção de urgência, auto-tagging, auto-summary

- **Auto-reply** — Rascunho de respostas automáticas

- **Detecção de spam** — Classificação integrada

- **Polling agendado** — Verificação periódica com cron-style scheduling

- **Roteamento por conta** — Regras por conta de email

- **Parsing de threads** — Reconstruction de conversas encadeadas

  

## Calendário

  

- **Local-first** — Calendário local com sincronização CalDAV

- **Provedores suportados** — Radicale, Nextcloud, Apple Calendar, Fastmail

- **ICS import/export** — Compatível com padrão iCalendar

- **Recorrência RRULE** — Expansão de regras de recorrência

- **Cores por calendário** — Organização visual

- **Contexto para o agente** — O agente lê/escreve eventos

  

## Documentos (Canvas/Artifacts)

  

- **Editor multi-abas** — Com histórico de versões

- **Formatos** — Markdown, HTML, CSV

- **Syntax highlighting** — Highlight.js integrado

- **Sugestões por IA** — Edições com tracked changes

- **Biblioteca** — Busca, organização e arquivamento

- **Processamento** — Extração de texto de PDFs, Office, EPUB (via markitdown)

  

## Notas & Tarefas

  

- **Estilo Google Keep** — Notas com checklist items

- **Lembretes** — Com datas de vencimento

- **Tarefas recorrentes** — Via expressões cron

- **Tarefas disparadas por evento** — Gatilhos automáticos

- **Webhooks** — Gatilhos externos para tarefas

  

## Galeria & Editor de Imagem

  

- **Biblioteca de fotos** — Com suporte a álbuns

- **Metadados EXIF** — Extração e exibição

- **Gerenciamento de imagens geradas por IA** — Organização automática

- **Editor de imagem em camadas** — Com rascunhos (drafts)

- **Ferramenta de assinatura** — Carimbo de assinatura digitalizada

- **Reconhecimento facial** — Identificação de pessoas em fotos

  

## MCP (Model Context Protocol)

  

- **Servidores MCP built-in:**

  - Email (ferramentas de email)

  - Memória (gerenciamento de memória persistente)

  - RAG (busca em documentos)

  - Geração de imagem (Diffusers)

- **Servidores configuráveis** — Suporte a stdio e SSE

- **OAuth** — Suporte a autenticação OAuth para conexões MCP

- **Per-server tool disable** — Lista de ferramentas desabilitadas por servidor

- **Browser MCP** — Automação de navegador via Playwright

  

## Autenticação & Segurança

  

- **Multi-usuário** — Contas com bcrypt + TOTP 2FA

- **Tokens de API** — Tokens Bearer scoped (`ody_`)

- **Sessões** — Gerenciamento com expiração

- **Webhook HMAC** — Assinatura de webhooks

- **CSP (Content Security Policy)** — Com nonces

- **Rate limiting** — Proteção contra abuso

- **Criptografia em repouso** — Senhas de email e dados sensíveis via Fernet

- **Prompt injection hardening** — Sanitização de entrada

- **Controle de acesso a ferramentas** — Admin vs non-admin

- **Internal tool loopback token** — Segurança do loop agente

  

## Configurações & Personalização

  

- **Temas** — Claro/escuro com CSS custom properties

- **Atalhos de teclado** — Navegação completa por teclado

- **Acessibilidade** — Módulo `a11y.js` dedicado

- **Idiomas** — Ícones e labels por idioma

- **Layout** — Gerenciamento de seções da sidebar

- **Tiling** — Gerenciamento de janelas (tile manager)

- **Modal snap** — Ajuste automático de modais

  

## Implantação

  

- **Docker Compose** — Odysseus + ChromaDB + SearXNG + ntfy

- **GPU NVIDIA** — Overlay CUDA

- **GPU AMD** — Overlay ROCm

- **GPU Apple** — Metal nativo (macOS)

- **Native Linux** — Venv Python direto

- **Windows** — PowerShell launcher

- **PWA** — Instalável como aplicativo

  

## CLI (36 Comandos)

  

```bash

odysseus              # Dispatcher principal

odysseus-backup       # Backup/restore

odysseus-calendar     # Gerenciar calendário

odysseus-contacts     # Gerenciar contatos

odysseus-cookbook     # Gerenciar modelos

odysseus-docs         # Gerenciar documentos

odysseus-gallery      # Gerenciar galeria

odysseus-logs         # Visualizar logs

odysseus-mail         # Gerenciar email

odysseus-mcp          # Gerenciar servidores MCP

odysseus-memory       # Gerenciar memória

odysseus-notes        # Gerenciar notas

odysseus-preset       # Gerenciar presets

odysseus-research     # Executar pesquisa

odysseus-sessions     # Gerenciar sessões

odysseus-skills       # Gerenciar skills

odysseus-tasks        # Gerenciar tarefas

odysseus-theme        # Gerenciar temas

odysseus-webhook      # Gerenciar webhooks

```

  

## Integrações

  

- **Claude Code** — Skill bundle para uso no terminal da Anthropic

- **Codex CLI** — Plugin de integração bidirecional

- **GitHub Copilot** — Autenticação OAuth device-flow para chat

- **Companion App** — Bridge para clientes na LAN (pairing via token)

- **CalDAV** — Sincronização bidirecional de calendário

- **CardDAV** — Sincronização de contatos

- **IMAP/SMTP** — Gerenciamento completo de email

- **ntfy** — Notificações push

  

## Áudio

  

- **Text-to-Speech (TTS)** — Síntese de fala

- **Speech-to-Text (STT)** — Transcrição de áudio (faster-whisper)

- **Voice Recorder** — Gravação direta no navegador

- **Controles TTS** — Play/pause/speed na interface do chat