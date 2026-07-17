# Wrapper — Gerenciador de Escrituras (Alicerce)

Página que embute o web app do Apps Script num `<iframe credentialless>`, contornando o roteador
de conta `/u/N/` do Google (a falha "Não foi possível abrir o arquivo" em navegador com 2+ contas
Google logadas). **Este é o link que a equipe deve usar** — nunca a `/exec` direta.

- `index.html` — o wrapper (embute a `/exec` prod do Escrituras via `credentialless`).
- `.nojekyll` — desliga o Jekyll no GitHub Pages (serve o HTML estático como está).

## Publicar (GitHub Pages — repo dedicado)

```bash
# de dentro desta pasta wrapper/
git init
git add index.html .nojekyll README.md
git commit -m "Wrapper Gerenciador de Escrituras"
git branch -M main
git remote add origin https://github.com/<user>/alicerce-escrituras.git
git push -u origin main
```

Depois: **Settings → Pages → Source = branch `main`, pasta `/ (root)`**. Repo precisa ser
**público** (Pages grátis). URL final: `https://<user>.github.io/alicerce-escrituras/`.

## Pré-requisitos no app (já feitos)

- Implantação: **Executar como: Eu** + **Quem pode acessar: Qualquer pessoa** (`ANYONE_ANONYMOUS`).
- `doGet` com `setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)` (`apps-script/Code.gs`).

Se a `/exec` de produção mudar (novo `deploymentId`), atualizar o `src` do iframe **e** o `href`
de fallback em `index.html`.
