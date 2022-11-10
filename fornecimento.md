# Fornecimento!

Sprig é um console de jogo físico projetado pelo Hack Club onde você escreve seus **próprios** [jogos!](https://editor.sprig.hackclub.com/) Com qualquer dispositivo de hardware, é óbvio que você comprar peças e montar sua placa.Então tudo que eu preciso fazer: é encontrar peças e um fabricante, comprar peças e pronto! Temos os Sprigs. Simples assim? não.

O fornecimento de peças hoje não é fácil não.*[Você](https://cloud-7qmksvucf-hack-club-bot.vercel.app/0screen_shot_2022-09-22_at_3.07.15_pm.png)  [já](https://cloud-1dg3e6nm9-hack-club-bot.vercel.app/0screen_shot_2022-09-22_at_3.07.42_pm.png)  [viu](https://cloud-1eysvgt04-hack-club-bot.vercel.app/0screen_shot_2022-09-22_at_3.20.50_pm.png)  [essas](https://cloud-4i20ywhg7-hack-club-bot.vercel.app/0screen_shot_2022-09-22_at_3.23.10_pm.png)  [faltas](https://cloud-bpjpduoo0-hack-club-bot.vercel.app/0screen_shot_2022-09-22_at_3.23.48_pm.png)  [absurdas](https://cloud-m5x11st40-hack-club-bot.vercel.app/0screen_shot_2022-09-22_at_3.24.33_pm.png)?*

Tá certo que muitas das peças comuns usadas por amadores em todo o mundo estão esgotadas. Ah, e os prazos de entrega são estipulados para vários meses, às vezes até mais de um ano antes do reabastecimento - então os bots analisam e apreendem o estoque, quando são reabastecidos.

Por mais que isso seja parcialmente atribuído a um problema de abastecimento (navios presos em canais, guerras, COVID, tensões geopolíticas), onde há disponibilidade de matérias-primas insuficientes, outro problema é a demanda. A demanda disparou para acompanhar o aumento na produção de dispositivos eletrônicos durante a pandemia.

O pessoal do Raspberry Pi não estava imune a isso. Você pode encontrar Raspberry Pis fora de estoque ou apenas cinco vezes seu preço original. Não conseguimos encontrar Raspberry Pi suficientes para prototipar, só para não dizer entregar centenas de kits Sprig em todo o mundo. Os Raspberry Pi são SBCs (Sigla para computador de placa única) com processadores ARM de 64 bits. Eles podem executar o Linux com um ambiente de desktop confortavelmente, e os novos suportam dois monitores 4K. OK né? Sim, eles apenas custam US $ 250 por unidade agora.

## Placa principal

Temos algumas opções de processador:

1. Poderíamos construir nosso próprio computador, com um Processador de Aplicativos Série NXP IMX6 ou uma CPU chinesa de fundo de quintal;

2. Poderíamos projetar nossa própria placa com um micro compputador;

3. Poderíamos usar uma placa que já existe, como a Raspberry Pi Pico;

A primeira opção é extremamente complicado. Os chips chineses não são ideais, e o chip NXP tem 289 pinos. Sim. E como mencionado anteriormente, não temos nenhum chip em estoque, além do RP2040. Então, naquele momento, era mais econômico comprar um Pico por US$ 5 do que projetar nossa própria placa, que tinha custos de prototipagem. Uma vantagem a mais  de uma placa de controle conectável foi a liberdade de usar o Sprig como um kit de desenvolvimento de hardware também.

Escolhemos o RP2040, que é um microcontrolador ARM dual-core de 32 bits projetado pela Raspberry Pi com uma velocidade máxima de clock de 133MHz. Pimoroni tinha uma [placa muito boa](https://shop.pimoroni.com/products/pimoroni-pico-lipo?variant=39386149093459) que era um pouco mais do dobro do preço, mas carregava a bateria para lítio-íon/polímero. Também íamos usar lítio, mas as regulamentações de envio de correio de lítio são muito rígidas e tornariam as exportações internacionais quase impossíveis. Além disso, a placa Pimoroni tem um requisito de tensão mais alto (3,3V->5V), comparado ao 1,8->5 no Pico. Existem apenas duas pilhas AAA no Sprig, conectadas em série, então usar a placa pimoroni exigiria uma bateria adicional, e não tínhamos espaço para isso.

Pico é o nome dele! Temos o Pico com cabeçalhos da [pishop.us](https://www.pishop.us/product/raspberry-pi-pico-h/) (o único lugar que os tinha em estoque a granel.)

Junto com o Pico para fixação da placa, usamos dois conectores fêmea genéricos de 2,54 mm de 20 pinos. 

Um console de jogos requer várias coisas - controles, tela, caixas de som e energia.

## Controles

A interface de controle do Sprig consiste em oito botões de doze milímetros. Nós achamos na [Adafruit](https://www.adafruit.com/product/1119). Também brincamos com botões menores e botões de montagem em superfície.

Nosso primeiro protótipo usou [botões de montagem em superfície de seis milímetros](https://www.adafruit.com/product/3983), que não encontramos muito estoque. Durante a montagem da placa, as peças devem estar em uma bobina inteira, e a Adafruit cortou as delas em tiras de 10, e não conseguimos encontrar o suficiente - a maioria das pessoas tinha talvez apenas algumas centenas. Eles também não eram muito sensíveis e você tinha que pressionar até o fim para registrar um clique.

Tentamos uma montagem de superfície Omron de seis milímetros de substituição [botão de perfil baixo](https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3FS-1000P/ 277812), que foi péssimo. 0/10. Também tentamos outro botão de montagem em superfície de seis milímetros, o que também não foi nada bom.

Finalmente decidimos pelo botão atual que temos agora. Ufa!

## Interruptores

O Sprig recebe energia de uma bateria dupla soldada na placa, então queríamos um interruptor manual que pudesse ser usado. Originalmente, optamos por um [interruptor](https://www.digikey.com/en/products/detail/w%C3%BCrth-elektronik/450404015514/9950812) da Wurth Electronik, que era muito caro, mas muito discreto. O controle deslizante seria colocado fora da placa. Infelizmente, isso foi fraco e metade deles quebrou em trânsito (com amortecimento!) A outra metade quebrou durante o uso normal. Não dá. Mudamos para um [interruptor do C&K](https://www.digikey.com/en/products/detail/c-k/JS202011SCQN/2094299). É montado na vertical e parece uma qualidade muito superior. Infelizmente, a C&K não obteve autorização de importação da China, então não conseguimos exatamente encomendar nossas peças para enviar para a China. De alguma forma, nossa empresa poderia fazê-los fazer isso se eles pedissem. Não tenho certeza do porquê disso.

## Bateria

Testamos vários suportes de bateria - AA e AAA de célula única e dupla. As peças têm cabos que podem cutucá-lo, por isso queríamos um suporte de montagem em superfície. Depois de finalizar o esboço da placa, decidimos por um [suporte AAA duplo para montagem em superfície](https://www.digikey.com/en/products/detail/keystone-electronics/1022/2137859) da Keystone. Isso foi ótimo: tentamos usar dois suportes AAA únicos, mas eles tinham estoque limitado. O suporte tinha duas variantes - uma com molas e outra com abas. A de mola não segurava bem as baterias e elas podiam cair com um pouco de tremor, ao contrário da com abas, que foi a que usamos.

## Auto-falante

O Sprig usa um [MAX98357AETE+T](https://www.maximintegrated.com/en/products/analog/audio/MAX98357A.html) da Maxim Integrated. É um amplificador PCM Classe D pequeno, mas com desempenho Classe A/B. Ele está conectado a um alto-falante CUI Devices [CVS-1508](https://www.digikey.com/en/products/detail/cui-devices/CVS-1508/2791828).

## Tela

Escolhemos um destaque da Adafruit - um [1,8" TFT](https://www.adafruit.com/product/358). Tem uma resolução de 160x128, cores de 18 bits e um driver de vídeo ST7735R. Pedimos à adafruit para soldá-los com pinos macho curtos. Eles são acoplados a um cabeçalho femêo “curto” de baixo perfil.

Você sabtem ideia de como é difícil encontrar um desses cabeçalhos? A Samtec tinha alguns que eram cabeçalhos curtos falsos, e as especificações da adafruit mostravam que as fabricantes eram de mais de uma década atrás, em algum lugar da Ásia. Acabamos de comprar conjuntos Adafruit [de cabeçalho curto](https://www.adafruit.com/product/4174) e eu mesmo os cortei com um alicate. Acho que fui bem e tive uma taxa de erro de 5%.

## Placa de circuito

As placas de circuito do Sprig foram encomendados à JLCPCB e ALLPCB. A JLC é mais barata e significativamente mais rápido (o serviço da DHL que eles usavam sempre chegou antes do previsto), mas a qualidade é terrível. ALLPCB tinha boas placas de circuito, só um pouco mais caros, mas eram propensos a atrasos - não era raro esperar quase uma semana extra por eles.

A montagem da JLC também foi muito mais barata. Para um lote de 5 Sprigs, me custaria US $ 100 na JLC e US $ 500+ na ALLPCB. Normalmente, pedimos JLC para protótipos rush e ALLPCB para validação de protótipos.

A máscara de solda preto fosco é a melhor amiga de qualquer designer de hardware, mas o verde sempre tem uma aparência clássica divertida. Então fomos com os dois. Também adorei a cor dourada da ENIG. Com testes para espessuras de substrato de 1,6 mm, 2,0 mm e 2,4 mm, concluímos que 1,6 mm era o mais fácil para os botões serem instalados. Informações para pedidos: 1,6 mm, 140 x 65 mm, preto fosco/verde brilhante, ENIG. Todo o resto é padrão.

## Proteção de inversão de polaridade

Depois de algumas duras lições, aprendemos que o Pico não tinha proteção de potência reversa. Isso significava que, se o Sprig estivesse conectado à bateria e ao USB simultaneamente, a energia do USB iria para a bateria. No começo, evitamos isso adicionando um diodo 1N4001 e, depois, um [diodo Schottky.](https://www.digikey.com/en/products/detail/kyocera-avx/SD1206T020S1R0/3749511)

## Outros passivos e LEDs

Todo o resto é bastante padrão. Você consegue dos EUA ou da China. Existem vários capacitores e resistores e dois LEDs de status. Estas peças podem ser encontradas em qualquer distribuidor de peças.

# Tradução

Escrito originalmente pelo [Hugo](https://github.com/Hugoyhu), que já é conhecido na comunidade do Hack Club por gerenciar o correio do Hack Club, este arquivo foi traduzido pelo [Lucas](https://github.com/LucasHT22) pouco conhecido na comunidade do Hack Club, talvez você já viu ele no Slack ou revisando jogos (PRs) no repositório do Sprig. Além disso ele é Diretor de Operações de um grupo de estudos na Zona Leste de São Paulo, também lidera um Hack Club.

Arquivo original [aqui](https://github.com/hackclub/sprig/blob/main/docs/SOURCING.md)
