---
name: planejar
description: Fluxo completo de planejamento — pesquisa, arquitetura e tarefas. Use quando quiser planejar uma feature nova ou um projeto inteiro antes de codar.
argument-hint: "[descrição do que quer construir]"
---

## Instruções

Execute o fluxo de planejamento em 3 etapas sequenciais:

### Etapa 1: Pesquisa (pesquisador)

Chame o agente **pesquisador** para buscar informações relevantes:
- Documentação de APIs envolvidas
- Bibliotecas recomendadas
- Padrões de implementação

Passe como contexto: o que o usuário quer construir ($ARGUMENTS).

### Etapa 2: Arquitetura (arquiteto)

⚠️ **ATENÇÃO**: O arquiteto usa Opus (caro). Antes de chamar, pergunte ao usuário:
> "Vou chamar o arquiteto (Opus, mais caro) pra criar o plano de arquitetura. Posso prosseguir?"

Se o usuário aprovar, chame o agente **arquiteto** passando:
- O pedido original do usuário
- O resultado da pesquisa (etapa 1)

Se o usuário recusar Opus, use o agente **planejador** (Sonnet) direto.

### Etapa 3: Tarefas (planejador)

Chame o agente **planejador** passando:
- O pedido original do usuário
- O resultado da pesquisa (etapa 1)
- O plano de arquitetura (etapa 2)

O planejador vai quebrar tudo em tarefas ordenadas com agente responsável.

### Entrega final

Apresente ao usuário:
```
# Plano: [Nome do que foi pedido]

## Pesquisa
[resumo do que foi encontrado]

## Arquitetura
[plano do arquiteto]

## Tarefas
[lista ordenada do planejador]

## Próximo passo
"Quer que eu comece a executar as tarefas?"
```
