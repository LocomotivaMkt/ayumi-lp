# Ayumi Rosa — landing page

Landing page da campanha de Outubro Rosa da Ayumi, construída a partir do
protótipo interno. Sem build e sem dependência instalada: `index.html` carrega
HTML, CSS e JS inline, e as imagens saem de `img/`.

Para ver localmente, abra o `index.html` no navegador.

**No ar (provisório):** https://ayumi.locomotiva.art.br — sem senha, mas com
`X-Robots-Tag: noindex`. Servido como estático pelo Caddy no Droplet, a partir
de `/srv/sites/ayumi`. Push na `main` **não** publica: o deploy é manual.

## Imagens

As catorze já entraram. Ficam em `img/`, treze em WebP e a capa de
compartilhamento em JPEG.

| Arquivo | Onde entra | Exibido | Origem |
|---|---|---|---|
| `marca-ayumi-rosa.webp` | marca da campanha, no hero | 500×177 | arte |
| `hero-composicao.webp` | figura do banner | 620×413 | foto recortada |
| `campanha-autocuidado.webp` | bloco "A campanha" | 900×1125 | foto |
| `autoexame-1-espelho.webp` | bloco "Comece no espelho" | 720×720 | ilustração |
| `autoexame-2-banho.webp` | região 06 — círculos | 720×720 | ilustração |
| `autoexame-3-deitada.webp` | região 05 — deitada | 720×720 | ilustração |
| `autoexame-axila-direita.webp` | região 01 | 720×720 | ilustração |
| `autoexame-axila-esquerda.webp` | região 02 | 720×720 | espelho da 01 |
| `autoexame-mama-inteira.webp` | região 03 | 720×720 | ilustração |
| `autoexame-parte-de-cima.webp` | região 04 | 720×720 | ilustração |
| `laco-outubro-rosa.webp` | ornamento no pé da caixa de texto | 190×151 | foto |
| `parceiro-benassi.webp` | slot de parceiro | 197×46 | logotipo |
| `parceiro-dois-cunhados.webp` | slot de parceiro | 176×52 | logotipo |
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

Em 16/09/2026 o dono trocou **três imagens de uma vez**, todas vindas de um
envio do cliente (`~/Downloads/wetransfer_shutterstock_2528042123-1-eps_…/`,
fora do repositório): o retrato do hero, a foto do bloco "A campanha" e o laço.

O arquivo do bloco "A campanha" mudou de nome junto — era
`campanha-tres-geracoes.webp`, com três mulheres de idades diferentes, e o nome
descreveria outra foto. Hoje é `campanha-autocuidado.webp`.

O **laço** deixou de ser desenho de linha e voltou a ser foto de cetim, mas
agora recortada: o PNG do cliente vinha com fundo branco e sombra projetada, e
o laço precisa de fundo transparente porque transborda a caixa e cai sobre o
rosa da seção. O recorte é por **saturação** (`HSV`, canal S acima de 18), sem
fechamento morfológico: o fundo e a sombra são cinza quase sem saturação e
saem, e o vão da laçada fica vazado de verdade. Fechar buracos tapa esse vão e
deixa uma mancha clara no meio do laço.

**O brilho do cetim precisa ser tingido**, e esse é o ponto não óbvio. No alto
da laçada a fita reflete a luz e fica quase branca na foto — sobre o fundo
branco do original isso não se nota, mas recortado sobre a caixa o laço parece
ter um pedaço faltando. O tratamento: dentro da máscara, todo pixel com
saturação abaixo de 48 e valor acima de 215 recebe a **matiz média do cetim**
(medida na própria foto) e tem a saturação elevada, preservando a luminosidade.
São ~67 mil pixels, quase todos na alça de cima. Sem isso, o laço chega no site
mordido — foi exatamente o que o dono viu em 16/09/2026.

É ornamento, então vai com `alt=""` e `aria-hidden="true"` — leitor de tela
pula.

Os originais em alta resolução não estão versionados; ficam em
`~/Desktop/fotos ayumi/` e em `~/Downloads/` na máquina de quem montou.

### Figura do banner

Em 16/09/2026 o cliente pediu, sobre a arte de campanha dele: "aproximar a moça
do título Ayumi rosa e trazer o laço rosa por trás". O dono do projeto mandou
uma **imagem única** com a moça e o laço já compostos, recortada, e ela entrou
no lugar do retrato emoldurado.

O que mudou no layout, e é o que atende o pedido:

- a imagem **não usa a classe `.foto`**, que traz borda arredondada e fundo. É
  `.hero__figura`, sem moldura, então o gradiente do banner passa por trás da
  moça e do laço como na arte da campanha;
- o **vão entre texto e figura caiu** de ~64px para ~20px (o `gap` da grade foi
  de `clamp(2rem,5vw,4rem)` para `clamp(.75rem,2vw,1.75rem)`) e a coluna da
  direita ficou um pouco maior — é isso que "aproxima a moça do título";
- a figura **sangra até a borda da janela**, não até a do container. O `.env`
  para em 1180px: sangrar só o `--gutter` deixava a figura ilhada no meio do
  rosa em telas largas, com um vazio enorme embaixo dela. A variável `--sangra`
  soma ao gutter o que sobra de cada lado (`(100vw - var(--larg)) / 2`), então
  ela encosta na borda em qualquer largura. O `.hero` ganhou `overflow:hidden`
  por causa disso. Em coluna única o sangramento é desfeito, senão ela encosta
  só na borda direita em vez de ficar centrada;
- o **teto de largura é folgado (1100px) de propósito**. Com teto apertado a
  figura encostava na borda mas ficava a 210px do texto em tela grande — o
  oposto do que o cliente pediu. Solta, ela preenche o vão inteiro entre o
  texto e a borda: em 1920px fica com 985px de largura e o banner sobe para
  ~820px de altura, que é o que faz a moça dominar o quadro;
- as colunas são `1.15fr 1.05fr`. **É a divisão das colunas que controla o
  tamanho da marca**, não o `max-width` dela: a arte ocupa 100% da coluna de
  texto, então enquanto a coluna não cresce o `width` não muda nada. Cheguei a
  apertar a coluna do texto (`.92fr`) para a figura crescer, mas aí os dois
  botões do banner empilhavam em 1180px; e cheguei a dar ao banner um container
  próprio, mais largo (`--larg: 1340px`), o que aumentava a marca mas
  desalinhava o banner das outras seções em 80px — o rótulo do hero começava
  antes do das demais. Redistribuir as colunas dá o mesmo ganho na marca (+12%)
  sem quebrar o alinhamento, ao custo de 6% na figura;
- **a figura encosta no topo da seção**, e isso não é decoração: a foto corta o
  alto da cabeça da moça. Solta no meio do banner, com fundo rosa acima, o
  corte lê como erro de recorte; encostada no topo, ele vira a borda da página.
  O `align-self:start` com `margin-top: calc(-1 * var(--hero-respiro))` anula
  exatamente o respiro superior da grade — as duas coisas usam a mesma
  variável de propósito, para não descolarem. **A margem negativa se repete
  embaixo** (`margin-bottom`), senão sobra uma faixa vazia entre o pé da figura
  e a faixa de números: com as duas, quando a figura é maior que o texto ela
  passa a definir a altura da seção e encosta nas duas bordas. Em 1920px a
  faixa que sobrava caiu de 81px para 1px. **Em coluna única essa margem de
  baixo é zerada** — ali ela puxava o texto para cima da figura, sobrepondo o
  rótulo. Em coluna única a figura sobe
  para antes do texto (`order:-1`) pelo mesmo motivo. **Se a foto do banner for
  trocada por uma sem corte no topo, esse alinhamento pode voltar ao centro.**

O arquivo é WebP com alpha a q=76: q=84 pesava 167KB e a diferença no rosto,
ampliada em 2x, não aparece. É a maior imagem da página e o LCP do banner, por
isso vai sem `loading="lazy"` e com `fetchpriority="high"`.

As faixas laterais vazias do PNG original foram cortadas (a arte ocupa de 110 a
1470 dos 1536px). Isso deixa o arquivo em 1360x1024, proporção 1,33 em vez de
1,50 — mais alto para a mesma largura de coluna, que é o que ajuda a fechar o
vazio embaixo. **Não há mais resolução disponível**: em telas grandes a figura
é exibida a ~985px a partir desses 1360, o que em tela retina fica levemente
suave. Se aparecer uma versão maior da foto, vale trocar.

## Marca da campanha

O "AYUMI rosa" do banner era tipografia — Archivo em caixa alta com o "rosa"
por cima em Caveat. Desde 15/09/2026 é **arte pronta**
(`img/marca-ayumi-rosa.webp`), enviada pelo dono do projeto: o "rosa"
manuscrito termina num traço que vira coração, e isso não se remonta com
fonte nenhuma. É o `<h1>` da página, com `alt="Ayumi Rosa"` — antes a página
não tinha `<h1>`, só os `<h2>` das seções.

O PNG original já vinha com transparência, mas com os pixels invisíveis em
preto por baixo. Reduzir direto misturaria esse preto na borda e deixaria
halo, então a arte foi redesenhada na cor dela (`#e0014a`, que é a do arquivo,
não o `--rosa` da página) guardando só o canal alfa. O original fica fora do
repositório, em `~/Downloads/ayumi-rosa.png` na máquina de quem montou.

No mesmo dia saiu o parágrafo de apoio do banner ("Durante todo o mês de
outubro..."), a pedido do dono: o banner ficou com rótulo, marca, frase e os
dois botões.

A fonte **Caveat** continua sendo carregada mesmo sem o "rosa" manuscrito —
ela ainda escreve o "autoexame" do título daquela seção.

### Texto do banner

A frase do banner segue a arte da campanha (`~/Downloads/Ayumi rosa.png` na
máquina de quem montou), não a paleta da página: **azul-marinho `#001b54`** com
o "amor." em **`#df0d6c`** e itálico, os dois medidos no arquivo. São os dois
únicos lugares com azul no site — `--marinho` e `--rosa-banner` existem só para
isso e não devem virar cor de interface.

As quebras de linha são forçadas com `<br>`, como na arte: "Cuidar de você /
é o maior gesto de amor." e "Juntos pela prevenção / e pelo cuidado." O tamanho
da frase é `clamp(1.3rem, 6vw, 2.15rem)`: o termo em `vw` segura o tamanho nas
telas médias, e o mínimo existe porque em 320px a segunda linha chegava a 11px
da borda e qualquer fonte de fallback a quebraria em três linhas.

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

São **dois**, definidos em 14/09/2026: **Benassi | SP** e **Dois Cunhados
Hortifruti**. Empresas, não instituições de saúde — em 11/09/2026 os slots
chegaram a receber FEMAMA, Hospital de Amor, ABRALE e Instituto Avon, e o dono
do projeto trocou os quatro por estes dois. Se aparecer referência a
instituição parceira em algum texto, é resíduo daquela versão. **O uso de marca
de terceiro depende de autorização formal de cada empresa** — não foi
verificado aqui.

Os dois vieram em PDF vetorial. Foram rasterizados a 300 dpi com
`pdftocairo -png -transp`, que preserva o fundo transparente e o antisserrilhado
do vetor (recortar o branco depois deixaria franja). Os originais ficam fora do
repositório, em `~/Desktop/` na máquina de quem montou: `LOGO NOVO PDF.pdf` e
`versão vetores.pdf`. Do segundo saiu a versão principal, a do "teste de
redução" — preto com `HORTIFRUTI` em verde, que é a de melhor contraste sobre
o branco do cartão.

Os dois têm proporções diferentes (4,3:1 e 3,4:1), então igualar a altura faria
um parecer maior que o outro. O que iguala é a **área aparente**: cada arquivo
foi gerado com cerca de 9.000 px² de área de exibição, pela fórmula
`altura = raiz(9000 / proporção)`. Logo largo sai mais baixo, logo compacto sai
mais alto, e o peso visual bate. Os arquivos estão gravados em 2x (o dobro da
medida de exibição, que é a que está nos atributos `width`/`height` do HTML),
para ficarem nítidos em tela retina.

Por isso o CSS **não** define altura para `.parceiro img`: quem manda é o
tamanho intrínseco de cada arquivo. Se substituir um logotipo, refaça a conta
em vez de esticar no CSS, senão aquele logo desequilibra a dupla. A grade são
duas colunas de no máximo 320px, centralizadas — com dois cartões, `1fr` cada
deixaria dois retângulos enormes com um logo pequeno no meio.

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
