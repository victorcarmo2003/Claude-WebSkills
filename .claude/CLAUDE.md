# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Idioma

Sempre responda em **português (pt-BR)** neste projeto.

## Sobre o projeto

**BLOXIA** — Plataforma SaaS para o ecossistema Roblox. Analytics de jogos com SDK Lua, marketplace de assets/serviços freelancer, sistema de investimentos em jogos, revenue sharing entre devs, e estimativas de ARPU. Website (Next.js) + App mobile (Android/iOS).

## Comandos

```bash
npm run dev           # Dev server na porta 4000
npm run build         # Build de produção
npm run type-check    # Checar tipos (tsc --noEmit)
npm run lint          # ESLint
npm run test          # Vitest (unit tests)
npm run test:e2e      # Playwright (E2E)
npm run db:push       # Prisma push schema pro banco
npm run db:generate   # Prisma generate client
npm run db:studio     # Prisma Studio (GUI do banco)
npm run db:seed       # Seed do banco
```

## Convenções

### Código
- TypeScript strict em todo o projeto
- Commits em inglês, Conventional Commits: `feat(escopo): descrição`
- Nomes de variáveis/funções em inglês, camelCase
- Nomes de tabelas/campos do banco em inglês, snake_case

### Arquitetura Next.js (App Router)
- Route Handlers → finos, só validam e delegam pra services
- Services em `lib/services/` → lógica de negócio
- Validação com Zod em `lib/validators/`
- Componentes são Server Components por padrão

## Regra de verificação obrigatória

**TODA implementação DEVE ser verificada antes de ser reportada como concluída.**

Após qualquer coder (frontend, backend, db) terminar de implementar, o fluxo obrigatório é:

1. **Build check** — Rodar `npm run build` e verificar se compila sem erros
2. **Type check** — Rodar `npm run type-check` se o build falhar pra isolar erros de tipo
3. **Lint** — Rodar `npm run lint` pra checar padrões

Se qualquer check falhar:
- O coder DEVE corrigir os erros antes de reportar como pronto
- NUNCA diga "implementação concluída" se o build não passa

### Verificação de páginas (frontend)
Após implementar ou modificar páginas, o coder-frontend DEVE:
1. Verificar se o build passa (isso compila todas as páginas)
2. Se o dev server estiver rodando, acessar a página via `curl http://localhost:4000/rota` pra confirmar que renderiza

### Verificação de API (backend)
Após implementar endpoints, o coder-backend DEVE:
1. Verificar se o build passa
2. Se o dev server estiver rodando, testar o endpoint com `curl`

## Agentes disponíveis

14 subagentes em `.claude/agents/`. Fluxo:
1. pesquisador (Sonnet) → pesquisa
2. arquiteto (Opus) → arquitetura (usar com moderação)
3. planejador (Sonnet) → features e tarefas
4. coder-db/backend/frontend (Haiku) → implementação
5. verificador (Sonnet) → **smoke test obrigatório pós-implementação**
6. debugger (Haiku) → testa funcionalidades e endpoints
7. performance (Sonnet) → **análise de performance** (Core Web Vitals, bundle, DOM, gargalos)
8. revisores (Haiku) → web, backend, db, vulnerabilidades
9. github (Haiku) → branches, commits, PRs

### Fluxo recomendado pra features
```
planejar → coder-* implementa → verificador testa → correções se necessário → performance (se tocou frontend) → revisar → github
```

### Quando rodar performance
- Após implementar/modificar **páginas ou componentes** que afetam UX
- Antes de PRs que tocam o frontend
- Periodicamente pra monitorar regressões
- Use `/performance /rota` pra analisar uma página específica

## Rotas do projeto

### Páginas (App Router)
- `/` — Landing page
- `/login`, `/callback`, `/login/age-error` — Auth
- `/analytics`, `/analytics/[gameId]` — Dashboard analytics
- `/games`, `/games/new`, `/games/[gameId]` — Gerenciar jogos
- `/marketplace`, `/marketplace/games`, `/marketplace/assets`, `/marketplace/services` — Marketplace
- `/invest`, `/invest/opportunities`, `/invest/[gameId]` — Investimentos
- `/revenue` — Revenue sharing
- `/settings` — Configurações
- `/orders/[id]`, `/orders/[id]/checkout`, `/orders/[id]/success` — Pedidos

### API (`/api/v1/`)
- `auth/` — roblox, callback, me, refresh, logout
- `games/` — CRUD de jogos
- `analytics/[gameId]/` — overview, timeseries, heatmap, sales
- `marketplace/` — listings, gigs, orders
- `invest/` — opportunities, interests
- `revenue/` — reports, access
- `estimates/[gameId]/arpu` — Estimativas ARPU
- `reviews/` — Avaliações
- `reputation/` — Reputação
- `webhooks/stripe` — Webhook Stripe
