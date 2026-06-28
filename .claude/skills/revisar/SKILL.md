---
name: revisar
description: Roda todos os revisores em paralelo (web, backend, db, vulnerabilidades) e entrega um relatório unificado. Use antes de commit ou PR pra garantir qualidade.
argument-hint: "[caminho ou descrição do que revisar]"
---

## Instruções

Rode os 5 agentes revisores **em paralelo** para o código indicado pelo usuário:

1. **revisor-web** — problemas visuais, HTML, CSS, layout, responsividade
2. **revisor-backend** — arquitetura, god components, tipagem, código duplicado
3. **revisor-db** — queries, migrations, modelagem, conexões
4. **revisor-vulnerabilidades** — segurança, OWASP Top 10
5. **performance** — Core Web Vitals, bundle size, elementos pesados, layout shifts (só se o dev server estiver rodando e houver mudanças em frontend)

Se o usuário especificou um caminho ou arquivo: passe como contexto pra cada revisor.
Se não especificou: revise os arquivos alterados (use `git diff --name-only` pra descobrir).

**Nota:** O agente performance só é chamado se `git diff --name-only` mostrar alterações em `src/app/`, `src/components/`, ou arquivos `.tsx`. Se não houver mudanças frontend, pule.

### Após receber os resultados

Junte os relatórios num único documento organizado:

```
# Revisão Completa

## 🟣 Visual / UI
[resultados do revisor-web]

## 🔴 Arquitetura / Código
[resultados do revisor-backend]

## 🟡 Banco de Dados
[resultados do revisor-db]

## 🔵 Segurança
[resultados do revisor-vulnerabilidades]

## ⚡ Performance
[resultados do performance — Core Web Vitals, gargalos, score]

## 📊 Resumo
- X problemas críticos
- Y problemas altos
- Z problemas médios
- Total: X+Y+Z findings
```

Se um revisor não encontrar problemas, mostre "✅ Nenhum problema encontrado" na seção dele.

Apresente o resultado final ao usuário em português.
