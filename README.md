# Ayumi Rosa — landing page

Landing page da campanha de Outubro Rosa da Ayumi, construída a partir do
protótipo interno. Sem build e sem dependência instalada: `index.html` carrega
HTML, CSS e JS inline, e as imagens saem de `img/`.

Para ver localmente, abra o `index.html` no navegador.

**No ar (provisório):** https://ayumi.locomotiva.art.br — sem senha, mas com
`X-Robots-Tag: noindex`. Servido como estático pelo Caddy no Droplet, a partir
de `/srv/sites/ayumi`. Push na `main` **não** publica: o deploy é manual.

## Imagens

Seis das sete já entraram. Ficam em `img/`, todas em WebP.

| Arquivo | Onde entra | Exibido | Origem |
|---|---|---|---|
| `hero-retrato.webp` | retrato do hero | 800×1067 | foto |
| `campanha-tres-geracoes.webp` | bloco "A campanha" | 900×1125 | foto |
| `autoexame-1-espelho.webp` | passo 1 — no espelho | 720×720 | ilustração |
| `autoexame-2-banho.webp` | passo 2 — no banho | 720×720 | ilustração |
| `autoexame-3-deitada.webp` | passo 3 — deitada | 720×720 | ilustração |
| `campanha-laco.webp` | still do laço, coluna de texto | 1000×750 | foto |
| `cta-maos.webp` | chamada final | 1200×675 | foto |
| `og-ayumi-rosa.jpg` | capa de compartilhamento | 1200×630 | foto |

As três ilustrações do autoexame chegaram com fundos diferentes entre si: uma
transparente e as outras duas em tons de rosa que não batiam. Todas foram
normalizadas para **`#fce8f2`** exato, e por isso são gravadas em **WebP sem
perda** — o WebP com perda desloca a cor chapada em ±1 e o fundo deixa de bater
entre as três. Se substituir alguma, mantenha os dois detalhes.

Os prompts que geraram cada imagem estão em
[`docs/prompts-imagens.md`](docs/prompts-imagens.md). Os das ilustrações 04, 05
e 06 repetem o mesmo parágrafo de estilo de propósito: é isso que faz as três
saírem coerentes mesmo geradas em sessões separadas.

Os originais em alta resolução não estão versionados; ficam em
`~/Desktop/fotos ayumi/` na máquina de quem montou.

## Logo

`img/logo-ayumi.webp` é a marca como veio, 300×145 com transparência.
`img/logo-ayumi-branco.webp` é a mesma arte pintada de branco puro, guardando o
canal alfa original para as bordas continuarem suaves sobre fundo escuro.

O topo troca de versão conforme o tema: a logo é vermelha e azul-marinho, e o
azul `#0d54c9` sobre o fundo quase preto do tema escuro fica ilegível, então no
escuro entra a negativa. O rodapé usa sempre a negativa, porque o fundo dele é
ameixa escuro nos dois temas.

As duas são clicáveis e levam para **https://www.ayumi.com.br/**, na mesma aba.
A marca tem outros dois domínios no ar (`ayumisupermercados.com.br`, também
loja online, e `redeayumi.com.br`, institucional em WordPress que hoje responde
com erro de PHP). Se o oficial for outro, é uma linha no `index.html`.

O ideal ainda é receber o **SVG** da marca: o arquivo atual é raster de 300px,
exibido a 150px, o que dá 2x e resolve, mas não escala além disso.

### Slots de parceiro

FEMAMA, Hospital de Amor, ABRALE e Instituto Avon continuam como caixas
tracejadas, esperando os arquivos oficiais. O uso de marca de terceiro depende
de autorização formal de cada instituição.

## Pendências antes de publicar

- [ ] Inserir os logos dos parceiros
- [ ] Confirmar o domínio oficial para onde a logo aponta
- [ ] Trocar a logo por SVG, se houver
- [ ] **Reconferir os números do INCA** na faixa de dados (73.610 / +95% / 1 em 12)
- [ ] Confirmar autorização de uso das marcas dos parceiros
- [ ] Definir o destino real dos CTAs (hoje são âncoras internas)
- [ ] Só então remover o `noindex` do vhost

## Estrutura do arquivo

`index.html` é um documento HTML completo: `<!doctype html>`, `<html lang="pt-BR">`,
`charset` e **`<meta name="viewport">`**. Não remova nenhum dos quatro.

Vale o aviso porque a primeira versão nasceu no formato de Artifact, onde esse
envelope é injetado por fora. Servido direto pelo Caddy, sem ele, o navegador
caía em *quirks mode* e o celular renderizava a página a 980px de largura,
encolhendo tudo.

O texto não usa travessão. Foi pedido assim; ao editar a copy, resolva com
vírgula, ponto ou dois-pontos.

A página **não tem JavaScript**. O `<script>` que existia servia só para copiar
os prompts dos mocks e saiu junto com eles.

`og:image` aponta para `og-ayumi-rosa.jpg` — JPEG de propósito: WhatsApp e
Facebook não geram prévia com WebP.

## Conteúdo de saúde

A página traz o aviso de que o autoexame não substitui a mamografia, a
recomendação de rastreamento do Ministério da Saúde e o disclaimer de conteúdo
informativo no rodapé. Esses três trechos não devem ser removidos em ajustes de
copy.
