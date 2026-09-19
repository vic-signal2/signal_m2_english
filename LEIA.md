# Por que o link do Cloudflare não abria, e como consertar

## A causa

O `index.html` do pacote era um **redirecionamento** para `/signal_m2_en.html`.
No seu repositório o arquivo chama-se `Signal_M2_English.html`. Nomes
diferentes, então o redirecionamento aponta para um arquivo que não existe, e
o Cloudflare devolve 404.

Os dois arquivos são o mesmo produto: mesmo conteúdo, mesmo md5.

## A correção

Suba o **`index.html`** desta pasta para a raiz do repositório. Ele não é um
redirecionamento: **é o produto**. Com isso a raiz do site abre o Signal
direto, e o nome do outro arquivo deixa de importar.

O `_headers` vai junto, para ninguém receber uma versão em cache.

## Se ainda não abrir, verifique nesta ordem

**1 · A pasta.** Se você descompactou o pacote e subiu a pasta
`signal-m2-english/` inteira, tudo está um nível abaixo da raiz. Ou mova os
arquivos para a raiz, ou, no Cloudflare, em *Settings › Builds & deployments*,
ponha `signal-m2-english` no campo **Build output directory**.

**2 · Maiúsculas e minúsculas.** O Cloudflare Pages diferencia. A URL
`/signal_m2_english.html` não abre o arquivo `Signal_M2_English.html`. Com o
`index.html` na raiz isso deixa de importar.

**3 · As configurações de build.** Para um site de arquivos estáticos:
- Framework preset: **None**
- Build command: **vazio**
- Build output directory: **/** (ou a pasta, conforme o item 1)

Se houver um comando de build configurado, o deploy falha e nada é publicado.

**4 · O deploy aconteceu?** Em *Deployments*, o último deve estar
**Success**. Se estiver *Failed*, o log diz o motivo em uma linha.

**5 · O .tar.gz não serve para nada no repositório.** O Cloudflare não
descompacta. Pode apagar.

## Como conferir que funcionou

Abra a raiz do site. Deve aparecer a abertura escura com as ondas e, no canto,
**SKIP**. Se aparecer uma página em branco com "Redirecting to Signal", o
`index.html` antigo ainda está lá.
