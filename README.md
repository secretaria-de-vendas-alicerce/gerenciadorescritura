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

## As pontes (postMessage, ORD-PERF-ESCRITURAS-002)

O `localStorage` de dentro do iframe credentialless some ao recarregar; o desta página fica. Por
isso o app guarda aqui duas coisas, e só responde à página do app (host de script deste projeto,
2 frames abaixo do iframe):

- `esc-codigo-get/set` (O1): o código das telas, gzip+base64. O app só o roda se o sha256 for o
  do build dele.
- `esc-sessao-get/set` (O3): o token da sessão (só UUID; vazio apaga). O app só mostra a tela
  depois de o servidor conferir o token (`retomar`); o Sair o apaga daqui e do servidor. Este
  storage é de todo o `github.io` da conta (aceito como no Imóveis, CONSELHO-015).

**Ordem de publicação:** este wrapper sobe ANTES do app que usa a ponte nova. Com o wrapper
velho, o app pede a senha (falha segura).
