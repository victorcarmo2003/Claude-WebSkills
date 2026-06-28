---
name: verificar
description: Roda o agente verificador pra testar build, tipos, lint e smoke test das páginas/endpoints. Use após qualquer implementação pra garantir que tudo funciona.
argument-hint: "[opcional: descreva o que foi implementado]"
---

## Instruções

Chame o agente **verificador** passando como contexto:

- O que foi implementado (se o usuário especificou em $ARGUMENTS)
- Se não especificou, use `git diff --name-only` pra descobrir o que mudou

O verificador vai:
1. Rodar `npm run build`
2. Rodar `npm run type-check`
3. Rodar `npm run lint`
4. Fazer smoke test das páginas (se o dev server estiver rodando)
5. Fazer smoke test dos endpoints (se o dev server estiver rodando)

### Após receber o resultado

Se **APROVADO**: informe ao usuário que está tudo funcionando.

Se **REPROVADO**: apresente a lista de problemas e pergunte:
> "Encontrei X problemas. Quer que eu chame o coder pra corrigir?"

Se o usuário aprovar, chame o coder apropriado (frontend/backend/db) passando a lista exata de erros pra corrigir. Após a correção, chame o verificador novamente pra confirmar.

Repita até APROVADO.
