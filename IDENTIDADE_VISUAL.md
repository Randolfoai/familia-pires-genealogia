# Identidade Visual — Genogramas Família Pires

Guia de referência para gerar os infográficos SVG de genealogia do livro "Infops livro 4 - Genealogia" (família Pires). Mantenha este arquivo atualizado sempre que uma nova convenção for definida — ele é o que garante consistência entre sessões e entre computadores diferentes.

Pasta do projeto: `D:\IABRACADABRA\0058 - Família Pires\Infops livro\4 - Genealogia`
Fotos: `D:\IABRACADABRA\0058 - Família Pires\Fotos do Livro` (também existe uma pasta antiga `Novas Fotos`, checar as duas)
Repositório git: `https://github.com/Randolfoai/familia-pires-genealogia` (branch `main`)

> **Nota sobre trocar de computador**: o repositório git cobre só a pasta `Infops livro\4 - Genealogia` (os SVGs/PDFs e este guia). A pasta de fotos (`Fotos do Livro` / `Novas Fotos`) fica **fora** do repositório e **não** é versionada — ela precisa ser levada separadamente. O usuário leva o diretório local de trabalho inteiro zipado (inclui a pasta de fotos) ao trocar de computador; ao chegar lá, extrair mantendo a mesma estrutura de pastas (`D:\IABRACADABRA\0058 - Família Pires\...`) para que os caminhos usados nos scripts continuem batendo sem precisar ajustar nada. Se o caminho no novo computador for diferente, avisar explicitamente para os caminhos serem atualizados.

---

## 1. Estrutura geral do SVG

- `viewBox="0 0 {largura} {altura}"` em pontos (pt). Conversão: **1cm = 28.34645669pt**.
- Largura quase sempre fixa em **11,3cm = 320.3pt**.
- Altura varia por infográfico: às vezes fixa (informada pelo usuário em cm), às vezes "livre"/calculada pelo conteúdo, às vezes "até um máximo".
- Fontes usadas (nomes de família, caem no fallback do sistema fora do Illustrator, mas os nomes ficam nos estilos):
  - `AGaramondPro-Bold` — negrito (nomes, títulos, cabeçalhos de seção)
  - `AGaramondPro-Italic` — itálico (subtítulos, cabeçalhos de mini-grupo, cônjuges quando não em negrito-itálico)
  - `AGaramondPro-BoldItalic` — negrito-itálico (cônjuges/notas na mesma caixa do nome)
- Cores: só preto e branco. Caixas com `fill:#fff; stroke:#000`. Linhas/espinha com `stroke:#000; fill:none`.

## 2. Elementos estruturais

### 2.1 Espinha vertical + caixas de nome (elemento base, usado em TODOS os infográficos)
- Espinha vertical em `x = 28.8`.
- Início da caixa em `x = 43.4`.
- Início do texto em `x = 53.2`.
- Largura padrão da caixa: `width = 256.9` (para caber dentro de 320.3pt com boa margem).
- Cada caixa é ligada à espinha por um traço horizontal (`<line>`), na altura vertical central da caixa.
- A espinha é um único `<path>` vertical que vai do topo do grupo até o último conector, tipo `M28.8,{y_inicio}v{comprimento}`.

### 2.2 Casal-tronco (2 caixas + forquilha)
**Regra de quando usar** (confirmada várias vezes pelo usuário): casal-tronco **com caixas** é o padrão default sempre que o casal é pai/mãe **imediato** da lista de nomes logo abaixo (nível "filhos de X e Y"). Só omitir quando o usuário disser explicitamente que a lista é só de irmãos, sem um casal-pai no topo.

- Duas caixas lado a lado, centralizadas horizontalmente no canvas, com um pequeno espaço entre elas.
- Tamanho típico das caixas: 62–80pt de largura × 14–18pt de altura, ajustar conforme o espaço disponível do infográfico (mais compacto = caixas menores).
- **REGRA (a partir de 2026-08-28): o nome dos pais dentro de cada caixa fica CENTRALIZADO** (`text-anchor="middle"`, x = centro da caixa; y = centro vertical da caixa + ajuste de baseline), não mais alinhado à esquerda com um padding fixo. Aplicar em todos os infográficos novos daqui pra frente.
- Forquilha: de cada caixa desce uma linha curta e depois converge diagonalmente até um ponto de junção central; desse ponto desce uma linha vertical que se conecta horizontalmente até `x=28.8` (a espinha), de onde começa a lista de filhos.
- Path típico: `M{cx1},{y+h} v{a} l{dx},{b}` (uma perna de cada caixa) + depois um path do ponto de junção até a espinha: `M{join_x},{join_y}v{...}H28.8v{...}`.

**Nível netos/bisnetos/trinetos**: o casal do topo aparece **só como título em texto** (nome + subtítulo "Netos de X e Y e seus respectivos cônjuges"), **sem as caixas+forquilha**. As caixas só aparecem no nível "filhos" (pais imediatos).

### 2.3 Subtítulo "(Denominados Troncos)" / "(Denominados Tronco-raiz)"
Do template mais antigo, mas o usuário pode pedir de volta pontualmente (ex: "Josina e Manoel (denominados Troncos)"). Quando pedido, colocar um texto itálico centralizado **acima** do casal-tronco. Não é o padrão default — só usar quando o usuário escrever isso explicitamente.

### 2.4 Título do infográfico
Formato padrão: **"Netos/Bisnetos/Filhos de X e Y e seus respectivos cônjuges"**, negrito, ~12.5–13px, alinhado à esquerda perto do topo (`x≈15`).
- Nível "filhos" com casal-tronco: **normalmente não tem título de texto separado** — o casal-tronco já contextualiza. (Só usar título quando pedido explicitamente ou quando o casal-tronco tiver sido omitido por instrução do usuário.)
- Nível "netos"/mais distante: título de texto é o padrão, já que não há casal-tronco com caixas.
- Sempre checar/corrigir typos comuns do usuário no título: "respetivos" → "respectivos", "e os respectivos" → "e seus respectivos".

### 2.5 Mini-grupos ("Filhos de X e Y:")
Dentro de um bloco de netos/bisnetos, cada família de origem vira um mini-grupo:
- Cabeçalho **itálico negrito**, ~8.2–9px: `"Filhos de X e Y"` (ou `"Netos de X e Y"` no nível seguinte). Sem dois-pontos é o padrão mais recente, mas o usuário às vezes usa. Seguir o texto dado, mas manter consistência dentro do mesmo infográfico.
- Cada mini-grupo tem sua **própria mini-espinha vertical** (não é uma espinha única para o infográfico inteiro).
- Nome em negrito + cônjuge em itálico (ver seção 3).

### 2.6 Cabeçalho de SEÇÃO ("Bisnetos de X e Y", "Trinetos de X e Y")
Fonte **negrito não-itálico**, maior que o mini-grupo (~10–11.5px), ex: `.sec`. Aparece como um título de nível acima dos mini-grupos "Netos de X e Y:" que vêm logo abaixo dele.
- Regra de quebra de página: uma seção **nunca** pode ficar sozinha no fim de uma parte — sempre grudada com o primeiro mini-grupo/conteúdo que vem depois dela.

## 3. Formatação de nome + cônjuge

**REGRA CRÍTICA (confirmada pelo usuário):** quando nome e cônjuge ficam na mesma linha, usar **exatamente um espaço antes e um depois da barra** (`" / "`).

**Técnica correta**: NUNCA estimar a posição X do cônjuge por fórmula (contagem de caracteres × largura estimada) — gera espaçamento irregular. Em vez disso, usar **dois `<tspan>` dentro do MESMO elemento `<text>`**, um para o nome (classe negrito) e um para `" / cônjuge"` (classe negrito-itálico), **sem atributo `x` no segundo tspan** — o SVG encadeia o texto automaticamente:

```xml
<text transform="translate(53.2 71.4)">
  <tspan class="nm" x="0" y="0">Nome da Pessoa</tspan>
  <tspan class="sp"> / Nome do Cônjuge</tspan>
</text>
```

### Variações de conteúdo na mesma linha
- **Sem cônjuge**: só o `<tspan class="nm">`.
- **Apelido**: `" (Apelido)"` em itálico, igual ao cônjuge, encadeado (ex: `"Nome (Apelido) / Cônjuge"`).
- **Nota** (solteiro(a), falecido(a) ainda criança, etc.): mesmo esquema, ex: `" (faleceu ainda criança)"`, `" / solteira"`.
- **2 casamentos**: quando a linha fica grande demais (nome + 2 cônjuges), usar **caixa de 2-3 linhas**: nome em negrito na 1ª linha, cada cônjuge em itálico numa linha própria abaixo, prefixado com `"/ "`. A altura da caixa cresce proporcionalmente (uma "LINE_H" por linha extra).

### Limite de largura de uma linha
Com caixa de 256.9pt de largura:
- Fonte ~8.2px: cabe até ~60-66 caracteres combinados (nome+cônjuge).
- Fonte ~7.0-7.5px: cabe até ~72-80 caracteres.
- Se ultrapassar, **reduzir a fonte** (nunca deixar o texto vazar da caixa) ou passar para formato de 2-3 linhas.
- **Sempre renderizar e OLHAR a prévia** antes de considerar pronto — overflow de texto não aparece só olhando os números/margens.

### Listas por vírgula (nível bisnetos/trinetos, quando o usuário não subdividiu por família)
Uma única caixa com vários nomes separados por vírgula, ex: `"Estefane, Ana Clara, Olivia"`. Se a linha for muito longa, quebrar em várias linhas dentro da mesma caixa (função `wrap_text`, ~48-58 caracteres por linha dependendo da fonte), aumentando a altura da caixa proporcionalmente. **Cuidado com a fórmula de altura**: para N linhas, `altura = 17.5 + (N-1)*LINE_H` (não `ROWH + (N-1)*LINE_H` — já causou overflow de texto pra fora da caixa numa sessão).

## 4. Fotos

- Sempre na parte de baixo do infográfico (ou de cada bloco "filhos", se o infográfico tiver vários blocos empilhados), abaixo de todas as caixas.
- Embutidas como base64 (`<image>` dentro de um `<g clip-path="url(#id)">`, com um `<clipPath>` retangular do tamanho exato da foto na página). `preserveAspectRatio` não é necessário — o clip + `transform="translate(x y) scale(s)"` já corta certinho.
- **1 foto sozinha**: largura calculada dinamicamente a partir do espaço vertical restante, mantendo a proporção real da imagem (nunca esticar/cortar errado).
- **Várias fotos lado a lado** (2 a 8): mesma ALTURA fixa para todas, LARGURA proporcional à razão de aspecto real de cada uma, centralizadas como grupo com um gap fixo entre elas.
  - **O gargalo é a LARGURA somada das fotos, não a altura do canvas.** Sempre calcular o `PHOTO_H` máximo a partir de `(largura_disponível - gaps) / soma_das_proporções(w/h)` **antes** de escolher um valor — senão a foto estoura o canvas horizontalmente.
  - **Com 6+ fotos pequenas, o gargalo real costuma ser a LEGENDA (texto), não a foto** — texto de legenda geralmente é mais largo que uma foto-retrato estreita. Calcular o espaço pela legenda mais longa, não só pela proporção da imagem. Se ainda assim não couber, encurtar a legenda pra só o primeiro nome (os apelidos completos já estão nas caixas acima) e avisar o usuário dessa escolha.
- **Legenda de foto individual**: nome centralizado (`text-anchor="middle"`) sob o centro horizontal da foto — nunca tentar reproduzir espaços literais que o usuário digitou tentando alinhar manualmente.
- **Legenda de foto de grupo** (várias pessoas na mesma foto): perguntar ao usuário se quer alinhamento posicionado (nome sob cada pessoa) ou lista corrida (mais simples, texto único abaixo da foto) — na prática o usuário sempre preferiu **lista corrida**.
- **Conferir SEMPRE o nome exato do arquivo** na pasta antes de embutir. Já aconteceu várias vezes de o nome/extensão que o usuário digitou não bater com o arquivo real (ex: pediu `39.jpg`/`40.jpg`/`41.jpg`, os certos eram `39-A.jpeg`/`40-A.jpeg`/`41-a.jpeg`; pediu `45-A.jpg`, existiam `45-A.jpg` E `45-a.jpeg` — arquivos diferentes; pediu `53.jpg`, existiam `53.jpg`, `53.png` E `53-a.jpg`; pediu `71.jpeg`, só existia `71.png`; pediu `91.jg` [typo], só existia `91.png`). **Sempre `ls` a pasta e, se houver ambiguidade real, perguntar ao usuário qual é o arquivo certo.**
- **Fotos "estilo documento" (3x4, retrato tipo carteira)**: usar quando o usuário pedir explicitamente (ex: "evitar corpo inteiro") OU sempre que a foto for claramente de corpo inteiro (proporção bem estreita/alta, tipo 0,35) mesmo sem pedido explícito — evita mostrar o corpo todo por padrão de qualidade. Caixa de proporção FIXA 3:4 (largura:altura = 0,75) igual pra todas as fotos do grupo, em vez de preservar a proporção nativa de cada imagem.
  - **Fórmula de escala CORRETA (tipo "cover", não só "fit-width")**: `scale = max(largura_da_caixa / largura_da_imagem, altura_da_caixa / altura_da_imagem)`. Usar só `largura_da_caixa/largura_da_imagem` FALHA quando a imagem é mais larga que alta (ex: retrato quase quadrado) — a imagem escalada fica mais baixa que a caixa e sobra uma faixa branca embaixo (bug já visto e corrigido numa sessão). Com a fórmula `max(...)`, a imagem sempre cobre a caixa inteira nos dois eixos.
  - A imagem é colocada com o topo alinhado ao topo da caixa (sem centralizar verticalmente) — o excesso de altura (corpo/pernas) é cortado pelo `clipPath`, mantendo rosto+ombros visíveis.
  - Quando a imagem escalada fica mais LARGA que a caixa (caso em que o fator decisivo foi a altura), centralizar horizontalmente: `x_offset = -(largura_escalada - largura_da_caixa) / 2` somado à translação X.

## 5. Fórmulas de escala (fonte × espaçamento)

Não existe um valor fixo único — calibrar pela **densidade de linhas** do infográfico:

| Cenário | Fonte nomes/cônjuges | ROWH | ROW_GAP | GROUP_GAP | SECTION_GAP |
|---|---|---|---|---|---|
| Muitas linhas (20-35+), várias páginas | 7.0–8.2px | 9.3–11 | 1.9–2.6 | 7.8–9 | 10–12 |
| Densidade média (10-20 linhas) | 8–9px | 11.5–14.5 | 2.6–3.4 | 9–11 | 12–15 |
| Poucas linhas (≤12), infográfico "solto" | 9–13px | 15–21 | 3–7.5 | 11–14 | 15–18 |

Título: 12.5–13px. Cabeçalho de mini-grupo (itálico): 8–9px. Cabeçalho de seção (negrito): 10–11.5px. Legenda de foto: 6.5–8px (menor quanto mais fotos couberem lado a lado).

**REGRA DE OURO para preencher a altura pedida (confirmada por edições do usuário no Illustrator em 2026-08-28):** depois de montar o SVG com constantes-base, sempre calcular `margem = altura_alvo - altura_do_conteúdo`. A margem final no rodapé deve ficar **pequena (~5-10pt), não folgada** — nunca aceitar uma margem de 30-80pt "porque cabe". Processo:
1. Gerar uma primeira passada com espaçamento normal (tabela acima) e medir a margem sobrando.
2. **Redistribuir TODA essa sobra de volta nos espaçamentos** — dividir a margem pelo número de gaps existentes (entre linhas, entre grupos, antes da foto, etc.) e somar essa fração a cada um deles — em vez de deixá-la acumulada só no final.
3. O aumento não precisa ser uniforme entre todos os tipos de gap: nas séries observadas, o usuário aumentou proporcionalmente mais os **gaps entre grupos/seções** (chegando a quase dobrar, ex. 7.6→13.6pt) do que o espaçamento *dentro* de um mesmo grupo entre uma caixa e outra (que mudou pouco, ex. 1.9→2.5pt). Ou seja: prioridade de onde jogar a sobra: (1) gap antes da foto, (2) gap entre grupos/seções, (3) gap entre linhas dentro do mesmo grupo — só mexer no nível 3 se ainda sobrar muito depois de esticar os níveis 1 e 2.
4. Recalcular e conferir que a margem final ficou pequena antes de entregar.

**Título curto e centralizado**: quando o título do infográfico for curto (ex. "Netos de X e Y", sem o sufixo "...e seus respectivos cônjuges"), CENTRALIZAR horizontalmente (`text-anchor="middle"` ou x calculado pra centralizar) em vez de alinhar à esquerda em x=15 — confirmado numa edição do usuário. Títulos mais longos que já ocupam boa parte da largura podem continuar alinhados à esquerda.

## 6. Infográficos divididos em várias partes (séries)

Quando o conteúdo não cabe numa altura só, o usuário pede pra dividir em N arquivos (`-parte1.svg`, `-parte2.svg`, ...).

**Regras confirmadas:**
1. **Nunca** escrever "continuação" ou qualquer rótulo desse tipo — a parte seguinte simplesmente resume o conteúdo.
2. Um **grupo** (cabeçalho "Filhos/Netos de X e Y" + suas linhas) **nunca** pode começar numa parte e terminar na outra — sempre inteiro numa parte só.
3. Um **cabeçalho de seção** (ex: "Bisnetos de X e Y") nunca fica sozinho no fim de uma parte — sempre grudado com o primeiro mini-grupo que vem depois.
4. Cada parte usa a MESMA constante de estilo/espaçamento das outras (consistência visual entre elas).
5. A(s) parte(s) intermediária(s) usam a altura fixa pedida (ex: 17,3833cm); a **última parte usa a altura real do conteúdo restante**, não a altura fixa (a menos que o usuário peça o contrário).
6. Quando uma seção não tem subgrupos nomeados e mesmo assim é grande demais pra uma parte (ex: uma lista corrida de 50+ trinetos sem divisão por família), quebrar em **chunks de tamanho fixo** (ex.: 15 nomes cada) e deixar cada chunk repetir o mesmo cabeçalho da seção — isso não é uma "continuação" (não usa essa palavra), é só o nome real da seção se repetindo, e permite que o empacotador automático funcione sem lógica especial.

**Técnica de geração recomendada para séries grandes**: em vez de calcular posições manualmente, modelar cada grupo/seção como um **bloco atômico** com uma altura (`content_h`) e um espaço-antes (`gap_normal`), e usar um **algoritmo guloso** que empilha blocos na página atual e abre uma nova página quando o próximo bloco não couber no orçamento (altura da página menos ~15pt de margem). Ver scripts de referência na seção 8.

**Quando o usuário pede pra "juntar"/redistribuir partes já feitas**: mover a fronteira do corte (sempre em limite de grupo inteiro) para a parte com mais espaço sobrando, e se precisar apertar um pouco, **escalar o espaçamento (não o texto)** por um fator (ex: 0.9) mantendo as mesmas fontes.

**Antes de dividir, sempre checar se realmente é necessário**: às vezes o conteúdo cabe inteiro numa única parte no estilo padrão da série — nesse caso, gerar 1 arquivo só e avisar o usuário que não precisou dividir, em vez de forçar uma divisão artificial.

## 7. Outros casos especiais

- **Casal sem filhos**: casal-tronco normal (caixas + forquilha) + uma nota em itálico centralizada abaixo, tipo "Não tiveram filhos" — sem espinha vertical (não há filhos pra conectar).
- **Infográfico sem casal-tronco identificado no texto, mas identificável pelo contexto** (o resto do texto menciona "Netos/Bisnetos de X e Y" mais abaixo): inferir e desenhar o casal-tronco mesmo que o usuário não tenha dado um cabeçalho explícito pro primeiro bloco.
- **Sem título nenhum**: quando o usuário pede explicitamente ("infográfico sem título"), omitir tanto o título de texto quanto qualquer casal-tronco — só a lista de caixas direto do topo.
- **Combinar dois infográficos já existentes num arquivo novo**: reaproveitar o conteúdo/estrutura de ambos exatamente como estão (não inventar títulos novos que não foram pedidos); usar as constantes de espaçamento mais compactas da série pra caber; reduzir a altura das FOTOS primeiro (são o maior consumidor de espaço vertical) antes de comprimir o texto.

## 8. Scripts de referência (Python)

Os infográficos são gerados via scripts Python que calculam as posições Y incrementalmente e escrevem o SVG final (muito mais confiável que posicionar à mão). Os scripts ficam em pastas de scratchpad temporárias por sessão (não versionadas no git) — se precisar recriar um, os padrões de nome de função/variável mais usados são:
- `emit_group(header_text, rows, header_y, header_class)` — desenha um mini-grupo completo (cabeçalho + espinha + caixas).
- `wrap_text(text, chars_per_line)` — quebra uma lista por vírgula em várias linhas dentro da mesma caixa.
- Modelo de "bloco atômico + empacotador guloso" para séries de N partes (ver seção 6).

Ao iniciar uma nova sessão de trabalho (inclusive em outro computador), **releia este arquivo primeiro** para não repetir os erros/decisões já resolvidos aqui.

## 9. Checklist rápido antes de entregar um infográfico

1. [ ] Largura/altura exatas conforme pedido (ou "livre"/"até X" conforme especificado).
2. [ ] Casal-tronco com caixas SÓ se for nível "filhos" (pai/mãe imediato da lista).
3. [ ] Nome + cônjuge com a técnica de tspan encadeado (nunca estimativa de posição X).
4. [ ] Nenhum texto vazando pra fora de uma caixa (conferir visualmente, não só pelos números).
5. [ ] Fotos: nome de arquivo conferido na pasta real; proporção da imagem respeitada; legenda não colide com a legenda vizinha.
6. [ ] Se dividido em partes: nenhum grupo cortado ao meio; sem a palavra "continuação"; última parte com altura real.
7. [ ] Margem sobrando não excessiva (ajustar espaçamento pra preencher bem o espaço pedido).
8. [ ] Renderizar com Edge headless e OLHAR a prévia antes de salvar como final.
9. [ ] Typos óbvios do usuário corrigidos com bom senso (avisar quando a correção não for óbvia).
