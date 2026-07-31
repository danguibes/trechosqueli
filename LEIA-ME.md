# trechos que li

Um arquivo só, com os 1.003 trechos dentro. Roda no seu computador com dois cliques, ou publicado
no GitHub para você abrir de qualquer máquina.

---

## 1. Rodando local (mais rápido de testar)

Ponha o `index.html` na mesma pasta que a pasta `posts/` do backup do Instagram:

```
minha-pasta/
├── index.html
├── revisoes.json
├── novos.json
└── posts/
    ├── 17843506016084697.jpg
    └── 202601/
        └── 18096544054718754.webp
```

Abra o `index.html` com dois cliques. Se as fotos não aparecerem, o app avisa e oferece um botão
para você apontar a pasta na mão.

---

## 2. Publicando no GitHub (para acessar de qualquer máquina)

Cabe tranquilo: o GitHub recomenda até 1 GB por repositório e o acervo inteiro dá uns 100 MB.

**Antes de tudo, uma coisa importante:** no plano gratuito, um site do GitHub Pages é **público**.
Qualquer pessoa com o endereço vê os 1.003 trechos e as fotos das páginas. Repositório privado com
Pages só existe nos planos pagos, e mesmo lá o site publicado continua aberto — o que muda é o
código-fonte. Se isso for um problema, pule para a seção "Outras formas de hospedar".

### Passo a passo

1. Crie um repositório novo no GitHub — por exemplo `trechos`. Marque como **público**
   (o Pages gratuito exige isso).

2. Na sua máquina, monte a pasta assim:

```
trechos/
├── index.html
├── revisoes.json
├── novos.json
└── posts/          ← a pasta inteira do backup
    └── meus/       ← nasce sozinha quando você adiciona um trecho
```

3. Suba:

```bash
cd trechos
git init
git add .
git commit -m "acervo de trechos"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/trechos.git
git push -u origin main
```

O push de ~100 MB demora alguns minutos. Se travar, veja "Se o push falhar" abaixo.

4. No GitHub: **Settings → Pages → Source: Deploy from a branch → Branch: `main` / `(root)` → Save**.

5. Em um ou dois minutos o site sai em `https://SEU-USUARIO.github.io/trechos/`.

### Deixando mais leve (opcional)

O `preparar-fotos.py` recomprime as fotos de ~100 MB para ~64 MB, o que faz o site abrir bem mais
rápido no celular. Renomeie a pasta `posts` para `posts-originais`, instale as dependências
(`pip install pillow pillow-heif`) e rode `python3 preparar-fotos.py`. Nasce uma pasta `posts` nova
com os mesmos nomes de arquivo — o `index.html` continua achando tudo.

### Se o push falhar

Arquivo individual acima de 100 MB o Git recusa — não é o caso aqui, a maior foto tem menos de 1 MB.
O que costuma acontecer é o push de muitos arquivos de uma vez estourar o tempo. Se der erro, suba
em duas partes:

```bash
git add index.html revisoes.json && git commit -m "app" && git push
git add posts && git commit -m "fotos" && git push
```

---

## 3. Como as correções viajam entre máquinas

Não existe servidor aqui — é um site estático. O arquivo **`revisoes.json`**, publicado ao lado do
`index.html`, é a versão oficial: o app carrega esse arquivo em qualquer aparelho que abrir o site.
O que você edita fica no navegador daquele aparelho até ser publicado.

Tem três jeitos de publicar, do mais prático ao mais manual.

### a) Botão "publicar no GitHub" (funciona no celular)

Aparece na barra lateral quando o site está no `github.io`. Uma vez configurado, é um toque: o app
grava o `revisoes.json` direto no repositório pela API do GitHub, e o botão mostra quantos trechos
estão esperando para subir.

Para configurar, em cada aparelho:

1. Vá em https://github.com/settings/personal-access-tokens → **Generate new token**
2. *Repository access*: **Only select repositories** → escolha só o `trechosqueli`
3. *Permissions* → *Repository permissions* → **Contents: Read and write**. Só isso.
4. Gere, copie, cole no campo "token do GitHub" da barra lateral e clique em
   **"guardar token neste aparelho"**.

O token fica guardado só naquele navegador e só dá acesso de escrita a esse único repositório. Se
perder o aparelho, revogue na mesma página do GitHub e pronto. Não use um token *classic* aqui: ele
teria poder sobre a conta inteira.

Depois de publicar, os outros aparelhos pegam a novidade em cerca de um minuto — o tempo do GitHub
Pages reconstruir o site.

### b) "copiar revisoes.json" (celular, sem token)

Copia o conteúdo inteiro para a área de transferência. No navegador do celular: abra o repositório
no GitHub, toque no `revisoes.json`, no lápis de editar, apague tudo, cole e confirme o commit.

### c) "baixar revisoes.json" (desktop)

Baixa o arquivo. Substitua o do repositório e dê push:

```bash
git add revisoes.json && git commit -m "revisões" && git push
```

Nos três casos o arquivo já sai mesclado — o que estava publicado mais o que você fez agora.

**Uma ressalva:** se você editar em dois aparelhos sem publicar no meio, o segundo a publicar
sobrescreve o primeiro. Na prática, publique ao terminar cada sessão.

O botão **"descartar mudanças locais"** joga fora o que você fez neste navegador e volta ao que está
publicado — útil quando você já publicou de outro aparelho.

---

## 4. O que o app faz

**Buscar** em tudo, ignorando acento (`misericordia` acha "misericórdia"). A tecla `/` pula para a busca.

**Filtrar** por tema, autor, obra e ano, combinando quantos quiser. Clicar no autor, na obra ou num
tema dentro de um trecho filtra por ele na hora.

**Excluir** trechos. Vão para a lixeira (`só mostrar → excluídos`), de onde dá para restaurar. O
"desfazer" aparece no rodapé logo depois de excluir.

**Repetidos**: encontrei 67 grupos de trechos repetidos — 134 fotos no total, quase todas o mesmo
arquivo publicado duas vezes. O filtro `só mostrar → repetidos` junta todos, e cada um desses
trechos ganha um botão **"Manter este, excluir a outra"** que resolve o grupo de uma vez.

**Corrigir** o texto com **negrito** (Ctrl+B), *itálico* (Ctrl+I) e <u>sublinhado</u> (Ctrl+U), além
do autor, da obra e dos temas. O botão "tirar formatação" limpa a seleção. Texto colado de fora
entra sem formatação, de propósito — para não trazer lixo de HTML junto.

**Compartilhar.** O `↗ compartilhar` de cada trecho abre um menu com:

- **WhatsApp** e **X / Twitter** — abrem já com o texto e o link do trecho preenchidos. No X o texto
  entra cortado em 200 caracteres, para caber no limite da plataforma.
- **Instagram, feed (4:5)** e **Instagram, stories (9:16)** — o app *desenha* o trecho como imagem e
  entrega para o compartilhamento do aparelho, que aí lista o Instagram. No computador, onde esse
  compartilhamento não existe, a imagem é baixada para você publicar pelo celular depois.
  O Instagram não aceita publicação direta a partir de uma página web — não existe link de intenção
  como o do WhatsApp — então esse é o caminho possível.
- **Compartilhar com foto** — aparece só no celular. Manda a foto original da página junto pela
  bandeja nativa, que serve para qualquer aplicativo instalado.
- **Copiar texto e link** / **Copiar só o link**.

O link aponta para o trecho específico (`.../#t482`). Quem abrir cai direto nele, com um selo
"trecho compartilhado" que se tira num clique para voltar ao acervo inteiro.

**Marcar** com estrela, **copiar** o trecho já com a atribuição, e **baixar** a seleção atual em
Markdown ou JSON.

---

## 5. Adicionando trechos novos

O botão **+ novo trecho**, no alto da tela, abre o formulário: a foto da página, autor, obra, data,
o texto e os temas. Funciona igual no computador e no celular — lá o campo da foto oferece a câmera,
então dá para fotografar a página e publicar na hora.

**Transcrever a foto.** O botão no rodapé do formulário tenta ler o texto da imagem, para você não
digitar tudo. Na primeira vez ele baixa um leitor de uns 15 MB; depois fica no cache do navegador.
Leva de dez a trinta segundos e erra algumas palavras — sempre confira antes de salvar.

**Onde as coisas ficam.** A foto é reduzida para 1200 px de largura e vai para `posts/meus/` no
repositório. O trecho em si entra no `novos.json`, ao lado do `revisoes.json`. Com um token guardado,
salvar já publica tudo: a foto, o `novos.json` e o `revisoes.json`, nessa ordem.

Sem token, o trecho fica guardado no aparelho com um selo **"não publicado"**, e sobe quando você
usar "publicar no GitHub". Não acumule muitos assim: as fotos ocupam espaço no navegador e ele tem
limite.

Trechos criados por você aparecem com o selo **"meu"**. O "corrigir" deles reabre este formulário —
inclusive para trocar a foto — em vez do editor simples dos trechos antigos.

---

## 6. Sobre os temas

Os 19 temas foram atribuídos automaticamente, por palavras-chave. Acertam a maioria, erram alguns, e
139 trechos ficaram sem tema nenhum — o filtro "sem tema" junta todos eles. É um rascunho de
catalogação: o app deixa você acrescentar, tirar e criar temas trecho a trecho.

---

## Outras formas de hospedar

Se a exposição pública incomodar:

- **Netlify** ou **Cloudflare Pages**: arrastar a pasta publica o site. Ainda é público, mas dá para
  pôr senha (o "password protection" da Netlify é pago; o Cloudflare Access tem plano gratuito para
  poucos usuários).
- **Nextcloud, Dropbox ou Google Drive**: guardam a pasta, mas nenhum roda HTML como site de verdade
  — você baixaria a pasta em cada máquina. Funciona, só não é tão prático.
- **Um repositório privado sem Pages**: você clona em cada máquina e abre o `index.html` local. Nada
  fica exposto, e o `git pull` sincroniza as revisões. É a opção mais fechada.
