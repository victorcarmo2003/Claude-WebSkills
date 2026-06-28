---
name: novo-componente
description: Cria um novo componente React/Next.js seguindo os padrões do projeto. Use pra scaffolding rápido de componentes.
argument-hint: "[NomeDoComponente]"
---

## Instruções

Crie um novo componente React seguindo os padrões do projeto.

### Antes de criar

1. Verifique os componentes existentes pra seguir o mesmo padrão:
   - Glob por `components/**/*.tsx` e `app/**/*.tsx`
   - Leia 1-2 componentes pra entender o estilo (Tailwind? CSS Modules? styled-components?)

2. Pergunte ao usuário se não ficar claro:
   - "É um Server Component ou Client Component?"
   - "Onde ele deve ficar? (components/ ou dentro de uma page?)"

### Ao criar

Chame o agente **coder-frontend** com instruções claras:
- Nome do componente: $ARGUMENTS
- Padrão de estilo do projeto (descoberto na etapa anterior)
- Server Component por padrão, `"use client"` só se necessário
- Props tipadas com TypeScript interface
- Exportar como named export

### Estrutura esperada

```
components/
  NomeDoComponente/
    NomeDoComponente.tsx    ← componente principal
    index.ts                ← re-export
```

Ou se o projeto usar flat structure (sem pasta por componente), siga o padrão existente.

Apresente o resultado ao usuário e pergunte se quer ajustar algo.
