---
name: deploy-check
description: Checklist de pré-deploy — roda build, testes, lint, audit de segurança e verifica se tudo está pronto pra produção. Use antes de fazer deploy.
---

## Estado atual do projeto
!`git status --short 2>/dev/null || echo "Não é um repositório git"`
!`cat package.json 2>/dev/null | head -5 || echo "Sem package.json"`

## Instruções

Execute os checks de pré-deploy em sequência e reporte o resultado:

### 1. Build
```bash
npm run build 2>&1 | tail -20
```
Se falhar → ❌ e mostre o erro.

### 2. Testes
```bash
npm run test 2>&1 | tail -20
```
Se não tiver script de test → ⚠️ "Sem testes configurados"

### 3. Lint
```bash
npm run lint 2>&1 | tail -20
```
Se não tiver script de lint → ⚠️ "Sem lint configurado"

### 4. Audit de segurança
```bash
npm audit 2>&1 | tail -15
```

### 5. Performance (se o dev server estiver rodando)
```bash
node scripts/performance.mjs --route=/ --no-login --runs=1 --out=performance-reports
```
Verifique o JSON gerado. Se LCP > 4s ou CLS > 0.25 → ⚠️ alerta.

### 6. Verificações extras
- Arquivos .env no git? `git ls-files | grep -i "\.env"` → se sim, ❌ CRÍTICO
- Console.log esquecido? `grep -rn "console.log" src/ --include="*.ts" --include="*.tsx" | head -10`
- TODO/FIXME restantes? `grep -rn "TODO\|FIXME" src/ --include="*.ts" --include="*.tsx" | head -10`

### Relatório final

```
# Deploy Check ✅/❌

| Check | Status | Detalhes |
|-------|--------|----------|
| Build | ✅/❌ | ... |
| Testes | ✅/❌/⚠️ | ... |
| Lint | ✅/❌/⚠️ | ... |
| Segurança | ✅/⚠️ | X vulnerabilidades |
| Performance | ✅/⚠️/❌ | LCP Xms, CLS X.XX |
| .env no git | ✅/❌ | ... |
| Console.log | ✅/⚠️ | X encontrados |
| TODO/FIXME | ✅/⚠️ | X encontrados |

## Veredicto: PRONTO / NÃO PRONTO pra deploy
```

Se tiver ❌ → "NÃO PRONTO" e liste o que precisa corrigir.
Se só ✅ e ⚠️ → "PRONTO (com ressalvas)" e liste os warnings.
