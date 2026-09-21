# Compliance e segurança — ayumirosa.com.br

**Data:** 21 de setembro de 2026
**Escopo:** o repositório `LocomotivaMkt/ayumi-lp` e o que está no ar em
`ayumirosa.com.br`, `www.ayumirosa.com.br`, `ayumi.locomotiva.art.br` e
`ayumi.159-65-47-58.sslip.io`.
**Método:** leitura do código, inspeção do servidor pela chave `deploy`,
consulta de DNS e verificação no navegador contra o domínio no ar.

> **Isto é auditoria técnica, não parecer jurídico.** As páginas legais
> escritas aqui são minuta: descrevem com fidelidade o que o código faz, mas
> quem tem jurídico deve revisar antes de a página ser liberada para busca.

---

## 1. O achado que mudou o enquadramento

A landing **não coleta nada**. Não tem formulário, não tem uma linha de
JavaScript, não grava cookie, não usa `localStorage` e não chama analytics,
pixel, mapa nem chat. Verificado no código e conferido no navegador contra o
domínio no ar: antes desta entrega existia **uma única** requisição a terceiro,
o CSS do Google Fonts.

Duas consequências práticas:

- **não cabe aviso de cookies** nesta página. Não há escolha a oferecer, e um
  banner aqui seria enfeite — a regra da ANPD pede banner quando existe cookie
  não-necessário, e aqui não existe cookie nenhum;
- a Política de Privacidade pôde ser **curta e verdadeira**, com uma linha de
  tratamento em vez de um modelo genérico cheio de seção que não se aplica.

O que sobra de tratamento de dado pessoal é o registro automático de acesso do
servidor, que guarda IP e user-agent. É sobre isso que a política fala.

---

## 2. O que já estava certo antes desta revisão

Vale registrar, porque não foi refeito:

- `lang="pt-BR"`, hierarquia de títulos sem pular nível, todas as imagens com
  `alt` (a decorativa com `alt=""` e `aria-hidden`), `:focus-visible` com
  contorno de 3 px, `prefers-reduced-motion` respeitado;
- disclaimer de conteúdo informativo no rodapé e o aviso de que o autoexame não
  substitui a mamografia;
- nenhum arquivo interno exposto: `/img/`, `.git`, `README.md` e o antigo
  `ayumi.caddy` respondem 404 (o `.caddy` foi tirado da raiz servida em
  18/09/2026);
- `www` em 301 para o apex, `X-Robots-Tag: noindex` nos três endereços, host
  desconhecido respondendo 404 no Caddy;
- nenhum segredo no repositório: sem `.env`, sem chave, sem token.

---

## 3. O que foi corrigido

### 3.1 Conteúdo de saúde — quatro correções

Estas são as mais graves do levantamento, porque a página é de saúde e estava
no ar.

| Antes | Depois | Por quê |
|---|---|---|
| `73.610` novos casos/ano | **`78.610`** | 73.610 é do triênio 2023-2025. Em **4 de fevereiro de 2026** o INCA publicou a *Estimativa 2026-2028*, com 78.610. Campanha de 2026 estava publicando número de três anos atrás |
| `+95%` de chance de cura | **`até 95%`** | O `+` afirma "mais de 95%". As fontes dizem o contrário: "as chances de cura **podem chegar a** 95%" (FEMAMA, via Febrasgo) e "chegam a **até** 95%" (Jornal da USP, Prof. José Roberto Filassi, FMUSP/Icesp). 95% é teto, não piso |
| `1 em 12` até os 75 anos, creditado ao INCA | **`71,57`** casos novos a cada 100 mil mulheres/ano | O INCA **não publica** risco em formato "1 em X" — ele publica taxa por 100 mil, e a página de incidência do próprio INCA não traz o "1 em 12". Era número sem fonte atribuído a quem não o publica. O 71,57 é o risco estimado que o INCA de fato divulga para 2026-2028 |
| "Confirme os números vigentes antes da publicação." | removido | Nota interna renderizada na página pública, visível para qualquer visitante |

A linha de fonte passou a creditar as **duas** origens separadamente, porque a
chance de cura não é número do INCA:

> Casos novos e risco estimado: INCA, *Estimativa 2026-2028: Incidência de
> Câncer no Brasil*, publicada em 4 de fevereiro de 2026. Chance de cura com
> diagnóstico precoce: FEMAMA, divulgada pela Febrasgo.

### 3.2 Páginas legais

Duas páginas novas, na stack do projeto (HTML estático, sem build), com o
mesmo cabeçalho, rodapé e tipografia da landing:

- **`privacidade/`** — controlador identificado, a única linha de tratamento
  que existe (IP e user-agent do log, 6 meses, art. 7º, II da LGPD c/c art. 15
  do Marco Civil), uma lista explícita do que a página **não** faz, a seção de
  transferência internacional, os direitos do art. 18 e como exercê-los;
- **`termos/`** — objeto, o caráter informativo do conteúdo em destaque no
  topo, regras de uso, propriedade intelectual, links externos,
  disponibilidade, responsabilidade sem afastar o CDC, e foro.

Ambas ficaram sob o mesmo `noindex` do resto da campanha, que sai junto quando
a página for liberada. **Página legal precisa ser encontrável** — quando o
`noindex` cair, cai para as três.

### 3.3 Identificação do controlador

O rodapé dizia apenas "© 2026 Ayumi". Passou a trazer, em todas as páginas:

> **Ayumi Supermercados Ltda.** — CNPJ 67.616.128/0001-55 — Estrada da Colônia,
> 95, Parelheiros, São Paulo/SP — CEP 04892-000

Mais os links para as duas páginas legais e para o canal do titular. A base do
rodapé diz, em uma linha, que a página não usa cookie e não pede dado — com
link para a política.

### 3.4 Google Fonts saiu; as fontes agora são servidas pelo domínio

A página puxava três famílias de `fonts.googleapis.com`, o que mandava o IP de
cada visitante para um servidor nos Estados Unidos **por causa de tipografia** —
tratamento e transferência internacional a declarar, sem nenhuma contrapartida.

As três fontes variáveis (Archivo 88 KB, Caveat 73 KB, Instrument Sans 29 KB,
subconjunto `latin`, todas sob SIL Open Font License 1.1) foram para `fontes/`,
com `css/fontes.css` e `preload` das duas usadas acima da dobra. Procedência e
licença documentadas em `fontes/LEIA-ME.md`.

Resultado conferido no navegador: **zero requisições externas** nas três
páginas. A página não conversa com ninguém de fora.

### 3.5 Cabeçalhos de segurança e prazo do log

O vhost não tinha **nenhum** cabeçalho de segurança. O arquivo pronto está em
`infra/ayumi.caddy` e acrescenta:

```
Strict-Transport-Security   max-age=31536000
X-Content-Type-Options      nosniff
X-Frame-Options             DENY
Referrer-Policy             strict-origin-when-cross-origin
Permissions-Policy          câmera, microfone, geolocalização, pagamento… tudo negado
Content-Security-Policy     default-src 'none'; style-src 'self' 'unsafe-inline';
                            img-src 'self'; font-src 'self'; base-uri 'none';
                            form-action 'none'; frame-ancestors 'none'
-Server  -X-Powered-By
```

A CSP é muito mais fechada que o padrão da casa, e pode ser: **esta página não
tem JavaScript**. Com `default-src 'none'`, script, `fetch`, iframe, objeto e
mídia ficam bloqueados por omissão — se um script entrar um dia, quebra aqui
primeiro, e é esse o objetivo. O `'unsafe-inline'` em `style-src` é inevitável
(o CSS da landing mora num `<style>` dentro do `index.html`); em `script-src`
seria teatro, e lá está `'none'`.

O bloco de log ganhou prazo:

```
roll_size 20MiB   roll_keep 20   roll_keep_for 4320h   # 180 dias = 6 meses
```

Sem isso, a política prometeria 6 meses e o arquivo cresceria indefinidamente.
4320 h são os 6 meses do art. 15 do Marco Civil da Internet.

O arquivo foi validado no próprio Droplet (`caddy validate`, Caddy v2.11.4):
**"Valid configuration"**. Ele **não** importa o snippet `(seguranca)` da
auditoria de 02/09/2026, que ainda não está no `Caddyfile` do servidor —
importar snippet inexistente faz o Caddy recusar a configuração inteira e
derrubaria os nove sites do Droplet.

### 3.6 Acessibilidade

- **Link "pular para o conteúdo"** nas três páginas (LBI, Lei 13.146/2015,
  art. 63). Verificado em Chromium headless: primeiro elemento tabulável,
  aparece no foco, e o Enter leva o foco para `#conteudo`;
- **contraste do rosa de interface**: `#E12C77` dava 4,35:1 com branco, abaixo
  do 4,5:1 do AA — e ele é fundo de texto em dois lugares (o botão primário, de
  0,9 rem em negrito, e a faixa inteira do CTA). Passou a `#E02472`: **mesmo
  matiz (335,1°), mesma saturação (75,1%)**, 1,8 ponto mais escuro, 4,51:1. O
  rosa medido na arte da campanha é o `--rosa-banner`, que não foi tocado.

Varredura final nas três páginas, em Chromium: **nenhuma falha de contraste**,
nenhuma imagem sem `alt`, nenhum pulo de nível de título, nenhum erro de
console, nenhuma resposta 4xx/5xx e todos os links internos respondendo 200.

### 3.7 `og:url` e `og:image`

Apontavam para `ayumi.locomotiva.art.br`. Passaram para `ayumirosa.com.br`.

---

## 4. Pendente — depende do cliente ou de passo root

### 4.1 O domínio perdeu SPF, MX nulo e não tem DMARC — **DNS, outra agência**

Quando a agência que administra o DNS criou os registros A, a zona foi
**recriada**: os nameservers mudaram de `a/b.auto.dns.br` para
`b/c.sec.dns.br` e a chave DS mudou (era 33150, hoje é 56728). No caminho se
perderam os dois registros que o Registro.br cria por padrão para domínio sem
e-mail:

```
MX  0 .                    -> sumiu
TXT "v=spf1 -all"          -> sumiu
```

Hoje `ayumirosa.com.br` não tem SPF, não tem DMARC e não tem MX. Na prática,
**qualquer pessoa pode forjar e-mail com esse remetente** — num domínio de
campanha de uma rede de supermercados, isso é um vetor de phishing pronto,
usando a marca do cliente.

O que pedir à agência (não mexer em nameserver, o DNSSEC continua ligado):

| Tipo | Nome | Valor |
|---|---|---|
| MX | `@` | `0 .` |
| TXT | `@` | `v=spf1 -all` |
| TXT | `_dmarc` | `v=DMARC1; p=reject; rua=mailto:<caixa que exista>` |

Se um dia existir caixa `@ayumirosa.com.br`, os três precisam ser reescritos,
não apagados.

### 4.2 Encarregado (DPO) não existe em lugar nenhum

O art. 41, §1º da LGPD exige que a **identidade e o contato do encarregado**
sejam públicos. A política da loja on-line da Ayumi
(`ayumi.com.br/politica/privacidade`) não nomeia ninguém, e esta também não
pôde nomear — não se inventa nome de encarregado.

A política publicada usa como canal do titular o
**`online@ayumi.com.br`**, que é o endereço que a própria Ayumi já publica
para pedidos de titular na política da loja, e cujo domínio tem MX ativo
(Outlook) e SPF — ou seja, **recebe mensagem de verdade**. Confirmado por
consulta de DNS.

Falta o cliente: (a) confirmar que esse é o canal certo para esta campanha e
que alguém lê; (b) indicar o encarregado, para o nome entrar na política.

### 4.3 Qual CNPJ é o controlador — decisão tomada, vale confirmar

Três CNPJs da mesma pessoa jurídica aparecem no caminho:

| CNPJ | O que é | Onde aparece |
|---|---|---|
| **67.616.128/0001-55** | matriz, Estr. da Colônia, 95, Parelheiros/SP | **escolhido** para o rodapé |
| 67.616.128/0003-17 | filial, Estr. Ecoturística de Parelheiros, 6651 | rodapé de `ayumi.com.br` |
| 67.616.128/0013-99 | filial, Av. das Nações Unidas, 22833, Jurubatuba | registrou o domínio no Registro.br |

Foi usada a **matriz**, porque a landing é institucional e não é de uma loja.
Todos os três foram conferidos no registro público da Receita e estão ATIVA.
Se o cliente preferir outro, é trocar em quatro lugares (rodapé do
`index.html`, das duas páginas legais e o corpo dos dois documentos).

### 4.4 Autorização de uso das marcas dos parceiros

Continua aberta, como já estava: Benassi | SP e Dois Cunhados Hortifruti
aparecem na seção de parceiros e a autorização de uso de marca não foi
verificada. É uma das duas razões pelas quais o `noindex` existe.

### 4.5 O passo root no Droplet

`/etc/caddy/sites/ayumi.caddy` é `root:root 644` e o usuário `deploy` não pode
escrevê-lo. O arquivo novo já está no servidor, em
`/home/deploy/ayumi.caddy.novo`, validado. O passo root está na seção 6.

### 4.6 Imagens geradas por computador

As catorze imagens da campanha são geradas, não fotografadas — inclusive as
"pessoas". Não há obrigação legal de declarar isso no Brasil hoje, mas num
conteúdo de saúde a ambiguidade importa: alguém pode ler uma ilustração como
paciente real ou como depoimento. Os Termos de Uso passaram a dizer, em texto,
que as imagens não retratam pessoas, pacientes nem casos clínicos reais, e que
nada nelas é depoimento ou promessa de resultado. Se o cliente preferir outra
redação, é a seção 5 dos termos.

---

## 5. O que **não** foi feito, de propósito

- **Não foi instalado aviso de cookies.** Ver seção 1.
- **Não foi publicado nada.** Tudo está em branch; o conteúdo no ar continua
  sendo o de antes desta revisão até haver OK.
- **O `noindex` não foi removido.** Ele segue nos três endereços e agora
  também nas duas páginas legais. Sai quando as pendências da seção 4
  fecharem — e sai dos **cinco** lugares no mesmo passo.
- **Não foi tocada a auditoria de 02/09/2026** (snippets `(seguranca)`,
  `(cache-estatico)`, páginas de erro). Esta entrega é auto-contida.
- **Nenhuma afirmação de conformidade** foi escrita em lugar nenhum: as
  páginas não dizem que o site "é conforme", "é certificado" ou "segue a ISO".

---

## 6. Como aplicar

### 6.1 Conteúdo do site (como `deploy`, sem root)

```sh
cd ~/Desktop/ayumi-rosa
rsync -av --delete --exclude .git --exclude infra --exclude docs \
  ./ droplet:/srv/sites/ayumi/
```

### 6.2 Vhost (como **root** no Droplet)

O arquivo já está em `/home/deploy/ayumi.caddy.novo`, validado com
`caddy validate`.

```sh
cp /etc/caddy/sites/ayumi.caddy /etc/caddy/sites/ayumi.caddy.bak-2026-09-21
install -m644 -o root -g root /home/deploy/ayumi.caddy.novo /etc/caddy/sites/ayumi.caddy
caddy validate --config /etc/caddy/Caddyfile --adapter caddyfile
systemctl reload caddy
```

### 6.3 Conferir no ar — pelo endereço que o cliente usa, seguindo redirecionamento

```sh
curl -sS -D - -o /dev/null -L https://www.ayumirosa.com.br/ | \
  grep -iE "^HTTP/|strict-transport|content-security|x-content-type|referrer-policy|permissions-policy|x-frame|x-robots"
curl -sS -o /dev/null -w "%{http_code} %{url_effective}\n" -L https://ayumirosa.com.br/privacidade/
curl -sS -o /dev/null -w "%{http_code} %{url_effective}\n" -L https://ayumirosa.com.br/termos/
curl -sS https://ayumirosa.com.br/ | grep -c "78.610"      # tem de ser 1
```

Deploy não se confirma pelo merge: só está no ar quando a URL pública devolve o
conteúdo novo.

---

## 7. Referências consultadas

- INCA, *Estimativa 2026-2028: Incidência de Câncer no Brasil* (04/02/2026) —
  síntese de resultados e tabela consolidada do Brasil
- INCA, página de incidência do câncer de mama (ainda com os dados de
  2023-2025, o que explica o número antigo)
- Febrasgo / FEMAMA (05/10/2022) e Jornal da USP (16/11/2021) — chance de cura
  com diagnóstico precoce
- Lei 13.709/2018 (LGPD), arts. 7º, 18 e 41; Lei 12.965/2014 (Marco Civil),
  art. 15; Lei 13.146/2015 (LBI), art. 63; Lei 8.078/1990 (CDC), art. 51
- Registro público da Receita Federal, para os três CNPJs
