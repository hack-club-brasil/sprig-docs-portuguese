# Vamos criar nosso primeiro jogo no [Sprig](https://sprig.hackclub.dev)!

Todos os jogos do Sprig se passam em uma grade de ladrilhos.
O Sprig usa sprites nesses ladrilhos para representar tudo dentro dos jogos. 

Vamos aprender como usar o Sprig criando um jogo simples.

### Criando um Jogador

Para começar, vamos associar um caractere a uma variável para que possamos usar na nossa legenda do sprite.

```js
const jogador = "j";
```

Agora podemos definir a imagem que queremos usar para nosso jogador como quisermos.

```js
const jogador = "j";

setLegend(
  [ jogador, bitmap`...`]
);
```

Clique no bitmap para abrir o editor de pixels e desenhar uma imagem para o nosso sprite.

![Screen Recording 2022-07-18 at 12 24 08 PM](https://user-images.githubusercontent.com/27078897/197604643-2a59cc85-5a07-446d-95b3-d9be844b62c0.gif)

O bitmap é armazenado como uma string. Para dar uma olhada nela, clique na pequena seta próxima ao número da linha. Você consegue minimizar isso clicando na mesma área novamente.

Vamos também adicionar alguns obstáculos, caixas e objetivos em nosso jogo.

```js
const jogador = "j";
const caixa = "c";
const objetivo = "o";
const parede = "p";

setLegend(
  [ jogador, bitmap`
................
................
................
.......0........
.....00.000.....
....0.....00....
....0.0.0..0....
....0......0....
....0......0....
....00....0.....
......00000.....
......0...0.....
....000...000...
................
................
................`],
  [ caixa, bitmap`
................
................
................
...888888888....
...8...8...8....
...8...8...8....
...8...8...8....
...8...8...8....
...888888888....
...8...8...8....
...8...8...8....
...888888888....
................
................
................
................`],
  [ objetivo, bitmap`
................
................
................
....444444......
...44....44.....
...4......4.....
...4.......4....
...4.......4....
...4.......4....
...44......4....
....4......4....
....44....44....
.....444444.....
................
................
................`],
  [ parede, bitmap`
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000`]
);
```

### Criando uma Fase

Vamos agora criar nossa primeira fase. Em breve vamos querer múltiplas fases, então vamos armazenar nossa fase atual em uma variável para manter todas elas em uma lista.

```js
let fase = 0;

const fases = [
  map`.`
];
```

Agora nós temos apenas uma fase. 
Clique em `map` para abrir o editor de mapas. 
__Certifique-se de apertar run__ para carregar a legenda antes de editar o mapa.

![Screen Recording 2022-07-18 at 3 17 36 PM](https://user-images.githubusercontent.com/27078897/197605676-4c1e7a9b-3acc-41f5-a958-8e15dc55ba91.gif)

Para definir o mapa, utilize `setMap`.

```js
let fase = 0;

const fase = [
    map`
      j.p.
      .cpo
      ....
      ....
    `
];

const faseAtual = fases[fase];
setMap(faseAtual);
```

### Adicionando Controles

Vamos adicionar alguns controles para nosso jogador.
Sprig tem 8 entradas no teclado: `w` (cima), `a` (esquerda), `s` (baixo), `d` (direita), e `i`, `j`, `k`, `l`.

Você pode fazer algo ao pressionar um botão, como:

```js
onInput("w", () => {
  getFirst(jogador).y -= 1;
});
```

Estamos utilizando a função `getFirst` para obter o sprite do nosso jogador.

Repetindo esse padrão, podemos adicionar movimentos para cima/baixo/esquerda/direita ao nosso jogador.

```js
onInput("w", () => {
  getFirst(jogador).y -= 1;
});

onInput("s", () => {
  getFirst(jogador).y += 1;
});

onInput("a", () => {
  getFirst(jogador).x -= 1;
});

onInput("d", () => {
  getFirst(jogador).x += 1;
});
```

![Screen Recording 2022-07-18 at 3 20 09 PM](https://user-images.githubusercontent.com/27078897/197607562-15d0146f-329c-4b90-ac91-584d1290528e.gif)


### Adicionando Comportamentos

Nós queremos que nosso jogador empurre caixas e não seja capaz de passar por paredes.

Vamos fazer o jogador, caixas e paredes como sólidos para que assim eles não consigam passar um pelo outro.

```js
setSolids([ jogador, caixa, parede ]);
```

Agora esses sprites não vão se sobrepor um ao outro.

![Screen Recording 2022-07-18 at 3 21 10 PM](https://user-images.githubusercontent.com/27078897/197606834-9c3c3e48-84bd-49a3-938e-43eea8ea05ce.gif)

No entanto, queremos que o jogador empurre caixas, e podemos definir esse comportamento com `setPushables`.

```js
setPushables({
  [ jogador ]: [ caixa ]
});
```

O argumento passado para a função de definir empurráveis (`setPushables`) significa que todo sprite do tipo `jogador` ou nesse caso `j` pode empurrar sprites do tipo `caixa` ou nesse caso `c`.

Vamos ver como isso está agora.

![Screen Recording 2022-07-18 at 3 22 43 PM](https://user-images.githubusercontent.com/27078897/197606970-76f14b26-b3b2-44dd-ac96-a3459613a7b9.gif)

### Condição de Vitória

Nós temos agora algumas mecânicas, mas não temos um jogo de verdade até que haja um objetivo. Vamos adicionar uma condição de vitória. O objetivo vai ser empurrar caixar nos objetivos verdes.

Nós podemos checar se todos os objetivos verdes estão cobertos após cada entrada e se eles estão, avançar para o próximo nível. Vamos utilizar a função `tilesWith` para nos ajudar com isso. Nós podemos passar os tipos de sprite para ela e ela vai retornar todos os ladrilhos que possuem cada tipo de sprite que passamos dentro dela.

```js
afterInput(() => {
  const numeroCoberto = tilesWith(objetivo, caixa).length;
  const numeroNecessario = tilesWith(objetivo).length;

  if (numeroCoberto === numeroNecessario) {
    // aumentar o número da fase atual
    fase = fase + 1;

    const faseAtual = fases[fase];

    // tenha certeza de que a fase existe antes e então defina o mapa
    if (faseAtual !== undefined) setMap(faseAtual);
  }
});
```

Vamos adicionar outra fase para ver isso em ação.

```js
const fases = [
  map`
j.p.
.cpo
....
....`,
  map`
j.p.
.cpo
....
..co`
];
```

![Screen Recording 2022-07-18 at 3 24 08 PM](https://user-images.githubusercontent.com/27078897/197607684-45683107-fb28-4900-95ff-cd0b1b69c1f5.gif)

### Polir

Agora vamos polir um pouco o nosso jogo.

Vai incomodar bastante se a gente precisar sempre recomeçar o jogo inteiro quando estivermos presos em uma fase.
Podemos corrigir isso adicionando a habilidade de reiniciar fases.

```js
onInput("j", () => {
  const faseAtual = fases[fase];
  if (faseAtual !== undefined) setMap(faseAtual);
});
```

Quando nosso jogo acabar, vamos avisar o jogador que ele venceu.

```js
afterInput(() => {
  const numeroCoberto = tilesWith(objetivo, caixa).length;
  const numeroNecessario = tilesWith(objetivo).length;

  if (numeroCoberto === numeroNecessario) {
    // aumentar o número da fase atual
    fase = fase + 1;

    const faseAtual = fases[fase];

    // tenha certeza de que a fase existe antes e então defina o mapa
    if (faseAtual !== undefined) {
      setMap(faseAtual);
    } else {
      addText("você venceu!")
    }
  }
});
```

### Hackeando Isso

Torne este jogo seu. Você pode tentar:

- Desenhar a arte do seu próprio personagem
- Adicionar mais fases ao seu jogo (mire em 10)
- Mudar as mecânicas do jogo
  - E se você pudesse empurrar paredes?
  - E se você pudesse se movimentar 2 blocos para a direita ou esquerda ao invés de um?
  - E se houvessem atiradores laser que destruíssem o jogador?
  - etc...

### Tudo Junto

Vamos dar uma olhada em todo o código junto agora.

```js
const jogador = "j";
const caixa = "c";
const objetivo = "o";
const parede = "p";

setLegend(
  [ jogador, bitmap`
................
................
................
.......0........
.....00.000.....
....0.....00....
....0.0.0..0....
....0......0....
....0......0....
....00....0.....
......00000.....
......0...0.....
....000...000...
................
................
................`],
  [ caixa, bitmap`
................
................
................
...888888888....
...8...8...8....
...8...8...8....
...8...8...8....
...8...8...8....
...888888888....
...8...8...8....
...8...8...8....
...888888888....
................
................
................
................`],
  [ objetivo, bitmap`
................
................
................
....444444......
...44....44.....
...4......4.....
...4.......4....
...4.......4....
...4.......4....
...44......4....
....4......4....
....44....44....
.....444444.....
................
................
................`],
  [ parede, bitmap`
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000
0000000000000000`]
);

let fase = 0;
const fases = [
  map`
j.p.
.cpo
....
....`,
  map`
j.p.
.cpo
....
..co`
];

const faseAtual = fases[fase];
setMap(faseAtual);

onInput("w", () => {
  getFirst(jogador).y -= 1;
});

onInput("s", () => {
  getFirst(jogador).y += 1;
});

onInput("a", () => {
  getFirst(jogador).x -= 1;
});

onInput("d", () => {
  getFirst(jogador).x += 1;
});

onInput("j", () => {
  const faseAtual = fases[fase];
  if (faseAtual !== undefined) setMap(faseAtual);
});

setSolids([ jogador, caixa, parede ]);

setPushables({
  [ jogador ]: [ caixa ]
});

afterInput(() => {
  const numeroCoberto = tilesWith(objetivo, caixa).length;
  const numeroNecessario = tilesWith(objetivo).length;

  if (numeroCoberto === numeroNecessario) {
    // aumentar o número da fase atual
    fase = fase + 1;

    const faseAtual = fases[fase];

    // tenha certeza de que a fase existe antes e então defina o mapa
    if (faseAtual !== undefined) {
      setMap(faseAtual);
    } else {
      addText("voce venceu!")
    }
  }
});
```

### Arquivo Original

Acesse o arquivo original [aqui](https://github.com/hackclub/sprig/blob/main/docs/docs.md)
