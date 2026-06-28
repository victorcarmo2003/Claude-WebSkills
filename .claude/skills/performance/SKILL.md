---
name: performance
description: Analisa performance de uma pagina — Core Web Vitals, bundle size, elementos pesados, layout shifts. Roda o script de coleta e chama o agente de performance pra diagnosticar gargalos.
argument-hint: "[rota, ex: /analytics] [--runs=3] [--no-login] [--width=1920]"
---

## Instrucoes

Chame o agente **performance** passando como contexto:

- A rota a analisar (se o usuario especificou em $ARGUMENTS, ex: `/analytics`)
- Se nao especificou, analise `/analytics` (pagina principal do dashboard)
- Qualquer flag extra que o usuario passou (--runs, --no-login, --width, etc)

O agente vai:
1. Verificar se o dev server esta rodando na porta 4000
2. Rodar `node scripts/performance.mjs --route=/rota` pra coletar metricas
3. Ler o JSON gerado com as metricas
4. Analisar o codigo fonte procurando gargalos
5. Gerar um relatorio completo com score

### Apos receber o resultado

Apresente o relatorio ao usuario com:
- **Score geral** (X/100)
- **Core Web Vitals** (FCP, LCP, CLS, TTFB)
- **Principais gargalos** encontrados
- **Acoes recomendadas** priorizadas por impacto

Se o usuario quiser corrigir os problemas, pergunte:
> "Encontrei X gargalos de performance. Quer que eu chame o coder-frontend pra otimizar?"

Se o usuario aprovar, chame o coder apropriado passando a lista exata de otimizacoes. Apos a correcao, rode `/performance` novamente pra medir a melhoria.

### Comparacao com reports anteriores

Se existirem reports anteriores em `performance-reports/`, mencione a evolucao (melhorou/piorou).

### Analisar multiplas paginas

Se o usuario pedir analise geral (sem especificar rota), analise as paginas mais criticas:
1. `/analytics` — dashboard principal
2. `/` — landing page (use --no-login)
3. `/marketplace` — marketplace (use --no-login)
