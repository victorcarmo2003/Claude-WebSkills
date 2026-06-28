---
name: screenshot
description: Tira screenshots de páginas do site usando Puppeteer. Login automático via dev-login. Precisa do dev server rodando.
argument-hint: "[rota, ex: /analytics] [--full] [--width=1920] [--height=1080] [--wait=3000]"
---

## Instruções

Tire screenshots de páginas do site usando o script `scripts/screenshot.mjs`.

### Pré-requisitos

1. Verifique se o dev server está rodando:
   ```bash
   curl -s -o /dev/null -w "%{http_code}" http://localhost:4000 2>/dev/null
   ```
2. Se não estiver (retorna erro), inicie em background:
   ```bash
   npm run dev &
   ```
   Espere até o server estar pronto antes de tirar o print.

### Como tirar o screenshot

Parse os argumentos do usuário (`$ARGUMENTS`):

- Se o argumento é uma rota (ex: `/analytics`), passe com `--route=`
- Se é uma URL completa, passe diretamente como argumento
- Se vazio, o script tira print de `/analytics` por padrão

**IMPORTANTE**: Sempre use `--route=` para passar rotas. NÃO passe a rota como argumento posicional começando com `/` pois o Git Bash do Windows converte isso pra path do filesystem.

Execute:
```bash
node scripts/screenshot.mjs --route=/rota --out=screenshots [opções]
```

**Opções disponíveis:**
- `--route=/rota` — rota do site (padrão: /analytics)
- `--full` — screenshot da página inteira (scroll completo)
- `--width=1920` — largura do viewport (padrão: 1920)
- `--height=1080` — altura do viewport (padrão: 1080)
- `--wait=3000` — esperar N ms antes do print (pra animações/dados carregarem)
- `--wait=.classe` — esperar um seletor CSS aparecer
- `--no-login` — não fazer login automático (pra páginas públicas como /login)

### Login automático

O script faz login automático via `/api/v1/auth/dev-login` antes de navegar pra página. Isso funciona apenas em dev (não em produção). Use `--no-login` pra páginas públicas.

### Após o screenshot

1. Leia a imagem gerada usando a ferramenta `Read` para mostrar ao usuário
2. Comente o que observou visualmente (alinhamentos, elementos cortados, cores, etc.)
3. Se o usuário pediu revisão, sugira melhorias específicas

### Exemplos de uso

```bash
node scripts/screenshot.mjs --route=/analytics --out=screenshots
node scripts/screenshot.mjs --route=/analytics --full --wait=5000
node scripts/screenshot.mjs --route=/login --no-login --width=375 --height=812
node scripts/screenshot.mjs http://localhost:4000/marketplace/games
```
