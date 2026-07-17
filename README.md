# Wrapper — Gerenciador de Escrituras (Alicerce)

Página que embute o web app do Apps Script num `<iframe credentialless>`, contornando o roteador
de conta `/u/N/` do Google (a falha "Não foi possível abrir o arquivo" em navegador com 2+ contas
Google logadas). **Este é o link que a equipe deve usar** — nunca a `/exec` direta.

- `index.html` — o wrapper (embute a `/exec` prod do Escrituras via `credentialless`).
- `.nojekyll` — desliga o Jekyll no GitHub Pages (serve o HTML estático como está).

## Publicar (GitHub Pages — repo dedicado)

Repo: **`secretaria-de-vendas-alicerce/gerenciadorescritura`** (público). Publicado via `gh`:

```bash
# de dentro desta pasta wrapper/
gh repo create secretaria-de-vendas-alicerce/gerenciadorescritura \
  --public --source=. --remote=origin --push
# habilitar Pages (branch main, root):
gh api -X POST repos/secretaria-de-vendas-alicerce/gerenciadorescritura/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

URL final: **`https://secretaria-de-vendas-alicerce.github.io/gerenciadorescritura/`** — é o link que a
equipe passa a usar (nunca a `/exec` direta). Pages leva ~1 min pra subir na 1ª vez.

## Pré-requisitos no app (já feitos)

- Implantação: **Executar como: Eu** + **Quem pode acessar: Qualquer pessoa** (`ANYONE_ANONYMOUS`).
- `doGet` com `setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)` (`apps-script/Code.gs`).

Se a `/exec` de produção mudar (novo `deploymentId`), atualizar o `src` do iframe **e** o `href`
de fallback em `index.html`.
