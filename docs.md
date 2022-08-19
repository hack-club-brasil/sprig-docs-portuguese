# O kit de ferramentas

O Sprig é um pequeno kit de construção para construir jogos baseados em ladrilhos. Ele foi feito pelo Hack Club, uma comunidade global de programadores adolescentes que acreditam que pessoas aprendem melhor fazendo coisas com as quais se importam e compartilhando-as com os outros.

Se essa é sua primeira vez utilizando o Sprig, tente jogar o jogo inicial. Após isso, cheque a galeria. Você também pode começar com um arquivo vazio e seguir [este tutorial](iniciando-no-sprig.md#vamos-criar-nosso-primeiro-jogo-no-sprig)

Se você ficar preso com algo, você pode falar com outras pesssoas da comunidade sobre o Sprig no [Slack do Hack Club](https://hackclub.com/slack).

Execute jogos clicando no botão `Run` ou pressionado `shift+enter`.

## Design de fases

Jogos do Sprig são criados a partir de grades de ladrilhos quadriculados.

### setLegend(bitmaps)

Diga ao Sprig que tipos de sprites (elementos) estão disponíveis em seu jogo.
Chaves de bitmaps (representações de elementos) devem ser um único caractere.
Nós recomendados armazenar chaves de caracteres em variáveis.

```js
const jogador = "j";
const parede = "p";

setLegend(
    [ jogador, bitmap`...` ],
    [ parede, bitmap`...` ],
);
```

Para criar um novo bitmap, digite: 

```
bitmap`.`
```

Esse símbolo (`) é chamado de backtick! Clique no botão em destaque "bitmap" para editar seu desenho.

A ordem que seus tipos de sprite aparecem em sua legenda (no `setLegend()`), também determina a z-order de como eles serão desenhados. Tipos de sprite que vêm primeiro serão desenhados no topo.

### setBackground(chaveDoSprite)

Desenha um bitmap como o plano de fundo do jogo:

```js
setBackground(chaveDoSprite)
```

Isso muda apenas visualmente o jogo.

### setMap(fase)

Desenhar uma fase é como desenhar um bitmap:

```js
map`...`
```

Os caracteres no mapa vêm a partir da sua legenda de bitmap.
As fases não precisam ser controlados a partir de uma legenda, você deve armazená-los dentro de uma variável. 
Você pode chamar a função `setMap` para limpar o jogo e carregar uma fase nova:

```js
const minhaFase = map`...`
setMap(minhaFase)
```

Você talvez queira ter múltiplas fases sobre controle por meio de um array (vetor) para mudar entre elas durante o jogo:

```js
const niveis = [
    map`...`,
    map`...`,
    // etc.
]
setMap(fases[0])

// Depois:
setMap(niveis[1])
```

### setSolids(chaveDoBitmap)

Sprites sólidos não podem se sobrepor um ao outro. 
Isso é útil para criar coisas como paredes:

```js
const jogador = "j";
const parede = "p";

setSolids([jogador, parede]);
```

### setPushables(mapeamentoComEmpurraveis)

Use `setPushables` para fazer que sprites possam empurrar outros em volta. O sprite na esquerda vai ser capaz de empurrar todos listados à direita.

```js
const jogador = "p";
const bloco = "b";

setPushables({ 
    [jogador]: [ bloco, jogador ] 
})
```

**Atenção!** Tenha certeza que tudo que você passar para `setPushables` é também um sólido, ou então os elementos não vão ser empurrados.

## Entrada do Usuário

O Sprite aceita 8 tipos de entrada  `w`, `a`, `s`, `d`, `i`, `j`, `k`, `l`.

Tipicamente `w`, `a`, `s`, `d` são usados como controles direcionais.

### onInput(tipo, callback)

Faça algo quando o jogador pressionar um controle:

```js
onInput("a", () => {
    // Movimente o jogador um ladrilho à direita
    getFirst(jogador).x += 1
})
```

### afterInput(callback)

Executa após o evento de entrada ter sido manuseado. Útil para checar coisas como se o jogador venceu:

```js
afterInput(() => {
    if (getAll(bloco).length > 0) {
        console.log("você venceu")
    }
})
```

## Sprites e Tiles (ladrilhos)

Cada ladrilho pode conter qualquer número de sprites empilhados um em cima do outro.

Sprites contêm:
```
{
    type
    x
    y
    dx
    dy
}
```

Você pode movimentar um sprite definindo `x` e `y`. 

A `bitmapKey` (chave de bitmap) também pode ser modificada para atualizar a renderização gráfica do elemento e regras de colisão que o sprite vai seguir.

```js
sprite.y += 1
sprite.type = "j"
```

`dx` e `dy` são limpos após `afterInput`. 
Eles podem ser usados para verificar se o sprite se moveu e quanto.

Você também pode remover um sprite utilizando `sprite.remove()`.

### getTile(x, y)

Retorna uma lista de sprites no ladrilho especificado.

### tilesWith(tipo, ...)

Retorna uma lista dos ladrilhos que possuem sprites de todos os tipos especificados na chamada da função.

```js
tilesWith(bloco)
```

`tilesWith` aceita múltiplos tipos de sprites.

```js
tilesWith(bloco, jogador, ...)
```

### addSprite(x, y, tipoDeSprite)

Cria um novo sprite de um determinado tipo.

### clearTile(x, y)

Remove todos os sprites do ladrilho especificado.

### getAll(tipo)

Retorna todos os sprites de um dado tipo. 
Se nenhuma chave de bitmap for especificada, retorna todos os sprites do jogo.

### getFirst(tipo)

Retorna o primeiro sprite de um determinado tipo. 
Útil quando você sabe que existe apenas um sprite daquele tipo, como no caso de um elemento de jogador.

É um atalho para `getAll(tipo)[0]`.

## Texto

### addText(string, opcoes = { x, y, color })

Você pode adicionar um texto com elementos opcionais `x`, `y`, e `color` (cor).

Por exemplo:

```js
addText("olá", { 
    x: 10, 
    y: 4, 
    color: [ 255, 0, 0 ] // vermelho
})
```

### clearText()

Limpa todo o texto na tela.

## Música e Efeitos Sonoros

O Sprig vem com um sistema de som embutido e sequenciador! Você pode usar isso para escrever sons de fundo, ou com batidas maiores criar efeitos sonoros.

Você pode criar um tune (tom) com a palavra `tune`. 
Como sempre, clique no botão para abrir a aba de edição.

```js
// Crie um tom:
const melodia = tune`...`

// Toque-o:
playTune(melodia)

// Toque-o 5 vezes:
playTune(melodia, 5)

// Toque-o até a morte do universo:
const playback = playTune(melodia, Infinity)

// Ou faça isso calar a boca antes:
playback.end()
```


## Depurando

Aba seu console do navegador para depurar.

Você pode checar o estado do seu jogo fazendo...

## Arquivo Original

Acesse o arquivo original [aqui](https://github.com/hackclub/sprig/blob/main/docs/docs.md)

## Expressões Idiomáticas

### Obter Vizinhos

### Encontrar Padrão

### Substituir

### Contar Overlaps