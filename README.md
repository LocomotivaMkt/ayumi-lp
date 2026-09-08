# Ayumi Rosa — landing page

Landing page da campanha de Outubro Rosa da Ayumi, construída a partir do
protótipo interno. Arquivo único: `index.html` (HTML + CSS + JS inline, sem
build, sem dependência instalada).

Para ver, é só abrir o `index.html` no navegador.

## Espaços de imagem

As sete imagens ainda não existem. Cada uma tem, no lugar dela, um bloco na
proporção final com o prompt de geração escrito dentro e um botão **Copiar
prompt**. No rodapé há um botão que copia o briefing dos sete de uma vez.

| # | Onde entra | Tipo | Proporção | Mínimo |
|---|---|---|---|---|
| 01 | Retrato do hero | foto | 3:4 | 1200×1600 |
| 02 | Três gerações — bloco "A campanha" | foto | 4:5 | 1400×1750 |
| 03 | Still do laço de cetim | foto | 4:3 | 1600×1200 |
| 04 | Autoexame, passo 1 — no espelho | ilustração | 1:1 | 1000×1000 |
| 05 | Autoexame, passo 2 — no banho | ilustração | 1:1 | 1000×1000 |
| 06 | Autoexame, passo 3 — deitada | ilustração | 1:1 | 1000×1000 |
| 07 | Mãos, chamada final | foto | 16:9 | 2000×1125 |

Os prompts de 04 a 06 repetem o mesmo parágrafo de estilo (cor, traço,
fundo, o que evitar). É isso que faz as três saírem coerentes mesmo geradas
em sessões separadas — não altere esse trecho sem alterar nos três.

Para trocar um mock pela imagem: substitua o `<figure class="mock">` por um
`<img>` com a mesma `aspect-ratio`, `width`, `height` e um `alt` descritivo.

## Slots de logo

Não levam prompt — são arquivos oficiais em SVG:

- topo e rodapé: marca Ayumi (horizontal e negativa) + assinatura da campanha;
- parceiros: FEMAMA, Hospital de Amor, ABRALE, Instituto Avon.

O uso de marca de terceiro depende de autorização formal de cada instituição.

## Pendências antes de publicar

- [ ] Gerar as 7 imagens e substituir os mocks
- [ ] Inserir os logos oficiais
- [ ] **Reconferir os números do INCA** na faixa de dados (73.610 / +95% / 1 em 12)
- [ ] Confirmar autorização de uso das marcas dos parceiros
- [ ] Definir o destino real dos CTAs (hoje são âncoras internas)

## Conteúdo de saúde

A página traz o aviso de que o autoexame não substitui a mamografia, a
recomendação de rastreamento do Ministério da Saúde e o disclaimer de
conteúdo informativo no rodapé. Esses três trechos não devem ser removidos
em ajustes de copy.
