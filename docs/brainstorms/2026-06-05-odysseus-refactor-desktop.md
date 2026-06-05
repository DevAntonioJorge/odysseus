# Odysseus: Refatoração — Desktop + Mobile Port: Discovery Notes

Date: 2026-06-05 · Goal: Decidir o futuro do fork — arquitetura para desktop + port mobile

## Summary / key decisions

- **Direção alterada no Q11:** Sai de cloud/web, entra **100% desktop + port mobile adiado**
- Motivação original (falta de deploy gratuito Python) **não se aplica mais** — desktop não precisa de deploy cloud
- Perfil: **projeto exploratório, sem prioridade alta** (prioridade 0)
- Shell tools: podem ser mantidas num app desktop
- **Stack escolhida no Q14: Tauri** — frontend SPA existente reutilizado  
- **Estratégia (Q15):** Tauri + Python sidecar — migração incremental. Backend Python roda como processo filho, Tauri commands substituem rotas gradualmente  
- **Rust:** pouca familiaridade, aprendizado ao longo do projeto  
- **Docker: eliminado** — todas as dependências Docker removidas:  
  - ChromaDB → vetores em SQLite + API de embeddings externa  
  - SearXNG → DuckDuckGo (API direta)  
  - ntfy → Notificações nativas do SO (Tauri API)  
- **Mobile:** adiado (foco exclusivo em desktop)

### Q2 — Destino: desktop ou web gratuito?

- Asked: Desktop app empacotado ou web app em plataforma free?
- Captured: Prefere web na nuvem, precisa de acesso mobile
- Flags: (none)

### Q3 — Acesso mobile: mesma rede ou remoto?

- Asked: Como imagina o acesso mobile?
- Captured: Não se interessa pelo modelo atual (server local + SPA browser). Isso direcionou para cloud.
- Flags: (none)

### Q4 — Cenários de acesso remoto

- Asked: Qual cenário te atrai mais? Cloud, tunnel, app nativo, PWA?
- Captured: Prefere cloud/web
- Flags: (none)

### Q5 — Experiência com JS/TS

- Asked: Nível e ferramentas que domina?
- Captured:
  - Frontend: React, Next.js, vanilla
  - Backend: Next.js, Hono, Fastify, Express
  - Deploy: Vercel, Netlify, Railway
- Flags: (none)

### Q6 — Next.js monolítico vs backend separado

- Asked: Next.js monolítico (API routes) ou frontend + backend separados?
- Captured: Vercel frontend + Cloudflare backend — visando flexibilidade e baixo custo
- Flags: (none)

### Q7 — Arquitetura Cloudflare

- Asked: Como distribuir a carga? Workers puro, Workers + Fly.io, ou sem agente?
- Captured: Preferiu comparar Convex vs Hono(CF) antes de decidir
- Flags: (none)

### Q8 — Convex vs Hono (Cloudflare)

- Asked: Next.js + Convex vs Next.js + Hono(Cloudflare)?
- Captured:
  - Convex: sem vector store nativo, timeout 60s em actions, storage 1GB, mas free tier funcional no começo. Sem cold start. Fica caro depois.
  - Hono(CF): D1 (5GB SQLite), Vectorize, R2 (10GB), Web Streams nativo. Mas **free tier inviável** (10ms CPU/request) — precisa de $5/mês para rodar LLM. Shell tool calls não funcionam em Workers.
  - Usuário não respondeu se topa os $5/mês — deixou em aberto para explorar Elixir
- Flags:
  - Custo mensal aceitável? → pendente

### Q9 — Elixir + Phoenix

- Asked: Qual familiaridade com Elixir/Phoenix?
- Captured:
  - Vantagens: LiveView substitui React/Next.js (sem npm, sem build). OTP/GenServer perfeito para agent loop. Oban para background jobs. Deploy gratuito via Fly.io (500h/mês), Render, Gigalixir.
  - Desvantagens: vector store precisa de pgvector ou ChromaDB externo. Embeddings precisam de API externa. Comunidade menor. Shell tools rodam via System.cmd (ok).
  - Deploy gratuito possível mas serviço dorme após inatividade.
  - Usuário não conhece Elixir — seria aprender do zero.
- Flags:
  - Disposição para aprender Elixir? → **Sim, disposto**
  - Custo mensal aceitável ($5/mês CF ou Fly.io)? → **$5/mês ok, mas prioridade 0 (exploratório)**
  - Shell tools (bash/python) essenciais? → **Não**

### Q10 — Fechamento de flags

- Asked: (flags em aberto)
- Captured:
  - Custo: $5/mês é aceitável, mas prioridade é zero (projeto exploratório, sem pressa)
  - Elixir: disposto a aprender
  - Shell tools: não são essenciais
- Flags: resolvidas

### Q11 — Redirecionamento: desktop + mobile

- Asked: (iniciativa do usuário)
- Captured: Mudou de direção — agora quer **100% desktop** e um **port para mobile**. Não deseja mais cloud/web.
- Flags:
  - Relação desktop-mobile? → pendente (pergunta seguinte)

### Q12 — Modelo desktop-mobile

- Asked: App único multiplataforma, desktop servidor + mobile cliente, ou dois apps separados?
- Captured: Opção 1 — app único multiplataforma
- Flags: (nenhum)

### Q13 — Flutter vs Tauri

- Asked: Flutter (Dart, reescreve tudo) vs Tauri (Rust + HTML/JS, reaproveita frontend)?
- Captured: Vamos **desconsiderar mobile por enquanto** — foco exclusivo em desktop agora
- Flags: (nenhum)

### Q14 — Stack desktop

- Asked: Tauri (reaproveita frontend + Rust backend) vs Electron (reaproveita tudo) vs Flutter (reescreve tudo) vs empacotar Python?
- Captured: **Prefere Tauri**. Reaproveita o frontend SPA existente, backend reescrito em Rust.
- Flags:
  - Escopo da reescrita do backend → pendente

### Q15 — Escopo da reescrita

- Asked: Tauri com sidecar Python (migração incremental) vs rewrite total Rust?
- Captured: **Opção A — sidecar Python + migração incremental**. Pouca familiaridade com Rust, vai aprender ao longo do caminho.
- Flags: (nenhum)

### Q16 — Dependências Docker

- Asked: O que fazer com ChromaDB, SearXNG e ntfy?
- Captured: 
  - **ChromaDB: eliminado** → substituído por algo embutido (SQLite + embeddings por API ou similar)
  - SearXNG e ntfy: (resposta parcial, perguntar na Q17)
- Flags:
  - SearXNG e ntfy → pendente (Q17)

### Q17 — SearXNG e ntfy

- Asked: SearXNG por DuckDuckGo? ntfy por notificações nativas do SO?
- Captured:
  - SearXNG → **DuckDuckGo** (API direta, sem Docker)
  - ntfy → **Notificações nativas do SO** (Tauri notification API)
- Flags:
  - Embeddings sem ChromaDB → pendente (detalhe de implementação)

## Open flags (pending input)

- **Embeddings sem ChromaDB:** usar API externa (OpenAI, Anthropic) ou SQLite FTS5 apenas para busca textual e dispensar vetores por enquanto? → decisão técnica a fazer na implementação
- **Rust:** pouca familiaridade, aprendizado durante o projeto
- **Mobile:** adiado, sem previsão
