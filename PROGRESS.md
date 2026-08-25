# Genogramas Pires — status do projeto

> Este arquivo existe para que qualquer sessão do Claude (em qualquer computador) consiga
> retomar o trabalho sem precisar de explicações extras. Leia isto antes de continuar.

## O que é

Série de infográficos SVG estilo "genograma" para o livro **"Infops livro 4 - Genealogia"**
da família Pires. Cada infográfico mostra um casal e seus filhos (ou netos/bisnetos),
seguindo um template visual fixo baseado em `Template.pdf` (nesta pasta).

## Padrão visual (regras do template atual)

- **Casal-tronco com CAIXAS** (duas caixas retangulares lado a lado + linha em "forquilha"
  até uma espinha vertical) é usado sempre que o casal é **pai/mãe imediato** da lista de
  filhos abaixo dele (nível "Filhos de X e Y").
- **Casal como TEXTO/TÍTULO simples** (sem caixas, sem forquilha) é usado quando o
  infográfico trata de gerações mais distantes, tipo "Netos de X e Y" ou "Bisnetos de X e Y"
  — nesse caso o casal-tronco aparece só como um título no topo, seguido de subtítulo
  itálico, e o conteúdo abaixo é dividido em mini-grupos por família de origem.
- Nunca escrever as palavras "pai"/"mãe" no infográfico — só os nomes.
- Cada filho é uma caixa retangular ligada por um traço horizontal a uma "espinha" vertical
  do lado esquerdo.
- Nome do filho em negrito; cônjuge(s) em itálico (na mesma linha via `/ Nome` quando cabe,
  ou em linha própria dentro da mesma caixa quando não cabe — a caixa cresce em altura
  conforme o número de linhas).
- Apelidos e observações (ex: "(Zé Pires)", "(faleceu ainda criança)") sempre em itálico.
- Quando o infográfico tem foto(s), elas ficam **sempre na parte de baixo**, abaixo de
  todas as caixas, embutidas como base64 dentro do próprio SVG (`<clipPath>` + `<image>`
  com `preserveAspectRatio="xMidYMid slice"`, ou sem slice quando a proporção da foto já
  bate com a caixa, para não cortar as pessoas).
- Quando uma única foto tem várias pessoas juntas, adicionar legenda pequena (~5.5–6px)
  abaixo da foto com o nome de cada pessoa posicionado (`<tspan x="...">`) alinhado à
  posição aproximada dela na foto.
- Dimensão do SVG (viewBox) é definida pelo usuário em cm por infográfico — converter
  cm → pt multiplicando por `28.34645669` (ex.: 11,3×10,5574cm = 320.3×299.3pt).
- Fórmulas de posição já testadas: espinha em `x=28.8`, início da caixa em `x=43.4`,
  início do texto em `x=53.2`, linha conectora na altura `boxY + altura/2`.
- Para infográficos com várias linhas/grupos empilhados, calcular as posições Y **via
  script Python** (gerar o SVG programaticamente) em vez de calcular manualmente —
  evita estourar a altura do canvas. Ver exemplos de abordagem nos scripts usados nesta
  sessão (não versionados no repo, ficaram em pasta temp local): `gen_netos.py`,
  `gen_mactha.py`.
- Para conferir visualmente um SVG sem precisar abrir o Illustrator: renderizar com Edge
  headless via PowerShell:
  `msedge --headless --disable-gpu --screenshot="saida.png" --window-size=640,600 "file:///caminho/arquivo.svg"`

## Infográficos já feitos (nesta pasta)

- `genogramaprudenciofausta.svg` — casal Prudêncio e Fausta, 9 filhos. Modelo original
  (template antigo, sem foto).
- `genogramaleonidasluiza.svg` / `.pdf` — casal Leônidas e Luiza ("Denominados Troncos"),
  8 filhos: Anibal, José (Zé Pires), Inês (Ineizinha), Etelvina, Fausta (Tota), Tomázia
  (Tomazinha), Dolores, Ricardo (Dr. Ricardo). **Pendência não resolvida:** confirmar com
  o usuário se "Inês (Ineizinha)" e "Etelvina" são de fato dois filhos separados.
  `genogramaleonidasluiza-1.svg` é uma variação/versão alternativa deste mesmo arquivo.
- `genogramaangelaluiza.svg` / `.pdf` — casal-tronco Anibal e Lucy + 5 filhos: Ângela
  Luiza (solteira), Leônidas (esposa Lúcia Maria), Beatriz (2 casamentos: Donizete e
  Divino), Lucylene (esposo Ruberval), Manoel (faleceu ainda criança, nota em itálico
  abaixo do nome). Uma foto de grupo (`Foto 31.png`, pasta "Novas Fotos" — não incluída
  neste repo) com Leônidas, Ângela, Beatriz e Lucylene juntos; legenda com os 4 nomes
  alinhados sob cada pessoa. Dimensão 11,3×10,5574cm.
- `genogramanetosanibal.svg` / `.pdf` — infográfico de netos + bisnetos de Aníbal e Lucy,
  retrato, 11,3×17,3833cm, sem foto. Aníbal e Lucy aparecem só como título (sem
  casal-tronco em caixas, por serem gerações distantes). Estrutura em blocos: "Netos"
  (3 mini-grupos: filhos de Leônidas/Lúcia, de Beatriz/Donizete, de Lucylene/Ruberval) e
  "Bisnetos de Aníbal e Lucy" (3 mini-grupos equivalentes um nível abaixo).
- `genogramamacthamarly.svg` / `.pdf` — casal-tronco José Pires/Raimunda (caixas, nível
  "filhos") + 3 filhos: Mactha (esposo Raime Wilmar, apelido "Goiano"), Marly (esposo
  Romário), Getulino (faleceu ainda jovem). Foto das duas irmãs Mactha e Marly juntas
  (arquivo real na pasta "Novas Fotos" é `35.jpg`, não `foto 35.jpg` como mencionado
  originalmente pelo usuário — mesma foto, nome diferente). Dimensão compacta:
  11,3×5,5cm.
- Pasta `Filhos de Leônidas e Luiza (denominados Troncos)/` — fotos de referência dos
  filhos de Leônidas e Luiza, usadas para montar os infográficos com foto (algumas já
  restauradas/tratadas, arquivos "magnific_...").

## Observação importante sobre fotos

As fotos-fonte (não os SVGs) ficam em `D:\IABRACADABRA\0058 - Família Pires\Novas Fotos\`
— essa pasta **não está neste repositório** (só a subpasta "4 - Genealogia" foi versionada).
Se o próximo infográfico precisar de foto, pergunte ao usuário o nome exato do arquivo e
confirme se ele está disponível no computador onde a sessão está rodando.

## Próximo passo esperado

Usuário vai fornecer a próxima lista de nomes/texto (casal + filhos, ou netos/bisnetos)
para gerar mais um infográfico no mesmo padrão. Perguntar sempre: (1) se há foto e qual o
arquivo/pasta, (2) a dimensão desejada em cm, (3) se há casal-tronco no topo (e se é
nível "filhos" — caixas — ou geração distante — só texto).
