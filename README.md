# Ayumi Rosa — landing page

Landing page da campanha de Outubro Rosa da Ayumi, construída a partir do
protótipo interno. Sem build e sem dependência instalada: `index.html` carrega
HTML, CSS e JS inline, e as imagens saem de `img/`.

Para ver localmente, abra o `index.html` no navegador.

**No ar (provisório):** https://ayumi.locomotiva.art.br — sem senha, mas com
`X-Robots-Tag: noindex`. Servido como estático pelo Caddy no Droplet, a partir
de `/srv/sites/ayumi`. Push na `main` **não** publica: o deploy é manual.

## Imagens

As quinze já entraram. Ficam em `img/`, catorze em WebP e a capa de
compartilhamento em JPEG.

| Arquivo | Onde entra | Exibido | Origem |
|---|---|---|---|
| `hero-retrato.webp` | retrato do hero | 800×1067 | foto |
| `campanha-tres-geracoes.webp` | bloco "A campanha" | 900×1125 | foto |
| `autoexame-1-espelho.webp` | bloco "Comece no espelho" | 720×720 | ilustração |
| `autoexame-2-banho.webp` | região 06 — círculos | 720×720 | ilustração |
| `autoexame-3-deitada.webp` | região 05 — deitada | 720×720 | ilustração |
| `autoexame-axila-direita.webp` | região 01 | 720×720 | ilustração |
| `autoexame-axila-esquerda.webp` | região 02 | 720×720 | espelho da 01 |
| `autoexame-mama-inteira.webp` | região 03 | 720×720 | ilustração |
| `autoexame-parte-de-cima.webp` | região 04 | 720×720 | ilustração |
| `laco-outubro-rosa.webp` | ornamento no pé da caixa de texto | 230×160 | line art |
| `parceiro-femama.webp` | slot de parceiro | 130×40 | logotipo |
| `parceiro-hospital-de-amor.webp` | slot de parceiro | 194×27 | logotipo |
| `parceiro-abrale.webp` | slot de parceiro | 133×40 | logotipo |
| `parceiro-instituto-avon.webp` | slot de parceiro | 109×48 | logotipo |
| `cta-maos.webp` | chamada final | 1200×675 | foto |
| `og-ayumi-rosa.jpg` | capa de compartilhamento | 1200×630 | foto |

As ilustrações do autoexame chegaram com fundos diferentes entre si: uma
transparente e as outras duas em tons de rosa que não batiam. Todas foram
normalizadas para **`#fce8f2`** exato, e por isso são gravadas em **WebP sem
perda** — o WebP com perda desloca a cor chapada em ±1 e o fundo deixa de bater
entre as seis. Se substituir alguma, mantenha os dois detalhes.

As quatro últimas chegaram a 1254×1254 com fundo ruidoso em torno de `#fae4ef`.
O tratamento foi: deslocamento aditivo da imagem inteira até a cor modal virar
`#fce8f2`, achatamento de tudo que ficou a menos de 14 níveis do alvo,
reescalonamento para 720×720 e um segundo achatamento com tolerância 3, porque
o reescalonamento reintroduz meio nível de ruído. O fundo final é `#fce8f2`
exato em ~90% dos pixels — o resto é o traço e a sua borda suavizada.

Os prompts que geraram cada imagem estão em
[`docs/prompts-imagens.md`](docs/prompts-imagens.md). Os das ilustrações do
autoexame repetem o mesmo parágrafo de estilo de propósito: é isso que faz a
série sair coerente mesmo gerada em sessões separadas.

O laço de `laco-outubro-rosa.webp` entrou em 11/09/2026 no lugar da foto do
laço de cetim, que saiu da página. Veio como PNG com o fundo já recortado, e o
recorte automático deixava halo cinza na borda do traço: o arquivo final é o
traço redesenhado na cor `--rosa` (`#e12c77`) guardando **só o canal alfa do
original**, que é o que faz a borda ficar limpa sobre o branco da caixa. É
ornamento, então vai com `alt=""` e `aria-hidden="true"` — leitor de tela pula.

Os originais em alta resolução não estão versionados; ficam em
`~/Desktop/fotos ayumi/` na máquina de quem montou.

## Logo

`img/logo-ayumi.webp` é a marca como veio, 547×217 com transparência, recortada
no contorno da arte (o PNG original tinha metade do quadro em espaço vazio).
`img/logo-ayumi-branco.webp` é a mesma arte pintada de branco puro, guardando o
canal alfa original para as bordas continuarem suaves sobre fundo escuro.

O topo usa a colorida e o rodapé usa a negativa, porque o fundo do rodapé é
ameixa escuro. A negativa também é o que resolveria o tema escuro, se ele
voltar um dia: o azul `#0d54c9` da marca sobre fundo quase preto fica
ilegível.

As duas são clicáveis e levam para **https://www.ayumi.com.br/**, na mesma aba.
A marca tem outros dois domínios no ar (`ayumisupermercados.com.br`, também
loja online, e `redeayumi.com.br`, institucional em WordPress que hoje responde
com erro de PHP). Se o oficial for outro, é uma linha no `index.html`.

O ideal ainda é receber o **SVG** da marca: o arquivo atual é raster de 547px,
exibido a 176px no topo e 208px no rodapé, o que dá quase 3x e resolve, mas não
escala além disso.

### Slots de parceiro

Os quatro logotipos entraram em 11/09/2026, enviados pelo dono do projeto:
FEMAMA, Hospital de Amor, ABRALE e Instituto Avon. **O uso de marca de terceiro
depende de autorização formal de cada instituição** — isso continua valendo e
não foi verificado aqui.

Os quatro têm proporções muito diferentes (o do Hospital de Amor é 7:1, o do
Instituto Avon é 2,3:1), então igualar a altura faria um parecer gigante e o
outro sumir. O que iguala a fileira é a **área aparente**: cada arquivo foi
gerado com cerca de 5.200 px² de área de exibição, pela fórmula
`altura = raiz(5200 / proporção)`. Logo largo sai mais baixo, logo compacto sai
mais alto, e o peso visual bate. Os arquivos estão gravados em 2x (o dobro da
medida de exibição, que é a que está nos atributos `width`/`height` do HTML),
para ficarem nítidos em tela retina.

Por isso o CSS **não** define altura para `.parceiro img`: quem manda é o
tamanho intrínseco de cada arquivo. Se substituir um logotipo, refaça a conta
em vez de esticar no CSS, senão aquele logo desequilibra a fileira. A grade cai
para duas colunas já em 1000px (e não em 820px, como as outras): abaixo disso o
cartão fica estreito demais para o logotipo do Hospital de Amor, que encolheria
e quebraria a equalização.

Os originais recebidos estão em `~/Desktop/TCC-ARRUMADOS/` na máquina de quem
montou — fora do repositório, como as demais fontes em alta resolução.

Em 11/09/2026 a seção passou a fechar a página, depois da chamada final, e
ficou **só com o título**: saíram o rótulo "Juntos somos mais fortes", o
parágrafo de apresentação e a nota de produção que pedia os logotipos em SVG
monocromático com altura óptica equalizada. Essa nota era o único lugar da
página que lembrava da autorização de uso de marca — por isso ela está
registrada aqui, e o pedido de arquivo continua valendo: SVG, versão
monocromática, altura óptica equalizada.

## Autoexame

A seção segue o material impresso da campanha: **seis regiões**, na mesma ordem
e com os mesmos nomes do folheto (debaixo do braço direito, debaixo do braço
esquerdo, toda a mama, por cima da mama, deite-se e toque-se, explore
realizando círculos). A observação no espelho não é uma das seis — ela vem
antes do toque, e por isso fica num bloco próprio acima da grade, e não dentro
dela.

As seis têm ilustração. **A região 02 é a 01 espelhada na horizontal** — não é
uma geração separada. Se um dia regerar a 01, a 02 sai dela de novo com um
`ImageOps.mirror`, senão as duas deixam de ser a mesma pessoa.

As quatro ilustrações das regiões 01 a 04 vieram numa geração só, em 11/09/2026,
e têm traço mais encorpado que a 05 (deitada) e a 06 (no banho), que são da
leva anterior. Convivem bem porque o fundo e a paleta são os mesmos, mas se
algum dia as duas antigas forem regeradas, o alvo é o estilo das quatro novas.

Os prompts de todas estão em [`docs/prompts-imagens.md`](docs/prompts-imagens.md).

## Pendências antes de publicar

- [ ] Inserir os logos dos parceiros
- [ ] Confirmar o domínio oficial para onde a logo aponta
- [ ] Trocar a logo por SVG, se houver
- [ ] **Reconferir os números do INCA** na faixa de dados (73.610 / +95% / 1 em 12)
- [ ] Confirmar autorização de uso das marcas dos parceiros
- [ ] Definir o destino real dos CTAs (hoje são âncoras internas)
- [ ] Só então remover o `noindex` do vhost

## Tema

A página é **clara e só**. Não há variante escura: nenhum bloco
`prefers-color-scheme`, nenhum `[data-theme]`, e `color-scheme: light` no
`:root` faz o navegador desenhar controles e barra de rolagem no claro mesmo
quando o aparelho está no modo escuro. Foi decisão do cliente. Verificado com
o sistema forçado em escuro: fundo `#FFF8FA`, texto `#3A1B2A`.

## Ordem das seções

Hero, números do INCA, A campanha, Autoexame, Sinais de alerta, chamada final,
Parceiros. Os parceiros fecham a página por decisão de 11/09/2026 — antes
vinham antes da chamada final. O menu do topo e a lista do rodapé seguem a
ordem de leitura, não a da grade.

## Estrutura do arquivo

`index.html` é um documento HTML completo: `<!doctype html>`, `<html lang="pt-BR">`,
`charset` e **`<meta name="viewport">`**. Não remova nenhum dos quatro.

Vale o aviso porque a primeira versão nasceu no formato de Artifact, onde esse
envelope é injetado por fora. Servido direto pelo Caddy, sem ele, o navegador
caía em *quirks mode* e o celular renderizava a página a 980px de largura,
encolhendo tudo.

O texto não usa travessão. Foi pedido assim; ao editar a copy, resolva com
vírgula, ponto ou dois-pontos.

A página **não tem JavaScript**. O `<script>` que existiu por algumas horas
servia só para copiar os prompts dos blocos mock do autoexame, e saiu junto com
eles quando as ilustrações entraram.

`og:image` aponta para `og-ayumi-rosa.jpg` — JPEG de propósito: WhatsApp e
Facebook não geram prévia com WebP.

## Conteúdo de saúde

A campanha é **exclusivamente de conscientização**. Em 11/09/2026 saiu a seção
"Como participar" — as quatro atitudes (compartilhar, marcar o exame, apoiar
quem está em tratamento, doar às instituições) — e com ela todas as chamadas
que levavam para lá: o item do menu, o segundo botão do hero, o botão da caixa
de rastreamento e o link do rodapé. A página informa e ensina o autoexame; não
convida a agir, doar nem aderir a nada. Ao escrever copy nova, mantenha essa
régua.

No mesmo dia saiu também a caixa **"E o rastreamento de rotina?"**, que ficava
no fim da seção de sinais e trazia a recomendação de mamografia do Ministério
da Saúde (a cada dois anos, dos 50 aos 69) mais a ressalva de que quem define é
o médico. Foi decisão do dono do projeto. Com ela saiu a frase "Nenhuma
campanha diagnostica ninguém. Quem faz isso é a consulta que você marca depois
de ler.", que estava ali desde a remoção da seção de participação.

A página continua trazendo o aviso de que **o autoexame não substitui a
mamografia** (na seção do autoexame), a orientação de procurar avaliação
profissional diante de qualquer sinal (na abertura da seção de sinais) e o
disclaimer de conteúdo informativo no rodapé. Esses três trechos não devem ser
removidos em ajustes de copy.
