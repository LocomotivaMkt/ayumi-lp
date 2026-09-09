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
| `cta-maos.webp` | chamada final | 1200×675 | foto |

**Falta a Imagem 03** (still do laço de cetim, 4:3). Ela continua na página como
espaço mock com o prompt dentro e botão de copiar — o mesmo formato que as
outras seis tinham antes de existirem.

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

## Slots de logo

Não levam prompt — são arquivos oficiais em SVG:

- topo e rodapé: marca Ayumi (horizontal e negativa) + assinatura da campanha;
- parceiros: FEMAMA, Hospital de Amor, ABRALE, Instituto Avon.

O uso de marca de terceiro depende de autorização formal de cada instituição.

## Pendências antes de publicar

- [ ] Gerar a Imagem 03 (still do laço) e substituir o último mock
- [ ] Inserir os logos oficiais
- [ ] **Reconferir os números do INCA** na faixa de dados (73.610 / +95% / 1 em 12)
- [ ] Confirmar autorização de uso das marcas dos parceiros
- [ ] Definir o destino real dos CTAs (hoje são âncoras internas)
- [ ] Só então remover o `noindex` do vhost

## Conteúdo de saúde

A página traz o aviso de que o autoexame não substitui a mamografia, a
recomendação de rastreamento do Ministério da Saúde e o disclaimer de conteúdo
informativo no rodapé. Esses três trechos não devem ser removidos em ajustes de
copy.
