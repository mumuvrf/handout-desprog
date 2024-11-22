Algoritmo de Programação Dinâmica: Mochila Binária
======

Situação-exemplo
---------

Anderson é um jovem sonhador que queria viajar o mundo inteiro sem gastar nenhum tostão. Para realizar seu objetivo, ele entrou de gaiato no navio S.S. Paulina. Porém, o capitão do navio logo identificou o intruso, e o pôs para descascar batatas no porão.

Um belo dia, entretanto, o navio foi invadido por piratas saqueadores, sob o comando de Durão Barba Ruiva. Durão, despreparado, tinha apenas uma velha mochila para carregar itens do barco, e Anderson teve uma ideia para, enfim, viajar de graça:

"Sua velha mochila suporta pouco peso. Por sorte, sei, de cor, peso e valor de cada item nesse barco. Encho a mochila com o que há de melhor se me levar a Xique-Xique, Bahia!"

Durão, então, respondeu: "Topo! Mas me perca um tostão e eu quebro a sua perna."

Anderson abriu um sorriso e saiu correndo pelo convés com a velha mochila. Porém, ele logo percebeu que não era tão fácil saber quais itens colocar na mochila e quais itens deixar para trás.

Suando frio e com medo de ter sua perna quebrada, nosso marinheiro resolveu pedir sua ajuda. Você, ávido por Desafios de Programação, aceitou. 

Agora é sua vez de ajudar: ***Como encontrar a melhor configuração de itens de forma a maximizar o valor total sem ultrapassar a capacidade da mochila?***


Primeiras impressões
---------

Anderson lhe entregou a mochila, que suporta **10 kg**. Você, então, pediu que a tripulação do S.S. Paulina organizasse os itens e lhe trouxessem.

Foi lhe entregue o seguinte:

* **Itens**:
    * **Prataria**, pesa 3 kg e vale R$ 100,00;
    * **Sonar**, pesa 5 kg e vale R$ 130,00;
    * **Rádio**, pesa 4 kg e vale R$ 90,00;
    * **Colete salva-vidas**, pesa 2 kg e vale R$ 60,00;
    * **Fogareiro portátil**, pesa 7 kg e vale R$ 150,00.

Vamos pensar por partes. Para encher a mochila, temos que ter um ponto de partida. Esse ponto de partida pode ser o primeiro item!

??? Atividade
Se você pudesse colocar um, e apenas um, item para colocar na mochila, qual seria? Qual o processo por trás dessa escolha?

::: Gabarito
Podemos pensar "Se o Durão Barba-Ruiva só pudesse sair daqui com um item, qual item o faria feliz?" Nesse caso, importa o item que é mais valioso.

Portanto, o mais intuitivo é fazer uma varredura pela lista de itens procurando esse item. Assim, chegamos ao **Fogareiro Portátil** que vale R$ 150,00.
:::
???

Colocar um item é fácil, pois há poucas informações a considerar. Assim, pudemos focar em uma única parte do problema: Maximizar o valor total na mochila. Porém, se tivermos que colocar dois ou mais itens, a escolha já pode não ser tão imediata.

??? Atividade
Selecione dois itens para colocar na mochila. Como você chegou a essa combinação? Que novos problemas surgiram?

::: Gabarito
Com dois itens, agora é necessário considerar não somente o peso e valor de cada item, mas a soma deles. Isso traz três novas situações:

- **A soma dos pesos dos itens ser maior que a capacidade da mochila**: Isso é o caso para a combinação Fogareiro Portátil + Sonar (7 + 5 = 12 kg), o que torna essa combinação inválida.
- **É preciso analisar várias informações de uma vez**: Você pode ter que somar os pesos e valores de vários itens, e ter controle das informações se torna difícil.
- **Pode existir uma outra combinação com mais itens que tenha maior valor**: Com dois itens, podemos maximizar o valor selecionando o Fogareiro Portátil e a Prataria (7 + 3 = 10 kg), totalizando 150 + 100 = R$ 250,00. Porém, com três itens, poderíamos combinar o Sonar, o Colete Salva-Vidas e a Prataria (5 + 2 + 3 = 10 kg), totalizando 130 + 60 + 100 = R$ 290,00.
:::
???

Quando consideramos, então, todos os itens a fim de encontrar uma combinação, temos que ter em mente a possibilidade de existir uma combinação alternativa com um valor maior. Isso nos leva a testar todas as somas de pesos e valores possíveis, o que, sem um método de organização e utilização dessas informações, se torna trabalhoso.

Como, então, podemos otimizar a análise de todas as combinações?

Descrição matemática do problema
----

Analisar todos os itens qualitativamente e simultaneamente é impossível. Precisamos considerar um item por vez, de maneira sequencial e quantitativa. Porém, como fazer isso?

Uma ideia é pensar que adicionar um item de determinado peso na mochila é equivalente a "trocar" a mochila por outra de capacidade reduzida. Por exemplo, se minha mochila tem capacidade 10kg e adiciono um item de 6kg, o que me resta é uma mochila de capacidade 4kg.

Desta forma, podemos olhar um item por vez e considerar: "Vale a pena colocar este item na mochila, dado o espaço que resta?".

Há duas possibilidades:

    **1. Colocar o item na mochila:** Nesse caso perdemos espaço da mochila que poderia ser usado para outros itens, mas ganhamos o valor do item.

    **2. Não colocar o item na mochila:** Em que deixamos de ganhar o valor do item, mas preservamos o espaço para usar com outros itens.

??? Atividade
Considere as duas combinações abaixo.

**Combinação 1:** Fogareiro Portátil + Prataria

**Combinação 1:** Sonar + Colete Salva-Vidas + Prataria

O que se pode afirmar sobre o valor da mochila em cada caso? Você diria que vale a pena colocar o Fogareiro Portátil na mochila?

::: Gabarito
Para a combinação 1 o valor contido na mochila é 150 + 100 = 250.

Para a combinação 2 o valor contido na mochila é 130 + 60 + 100 = 290.

Não vale a pena colocar o Fogareiro Portátil na mochila, pois **o valor contido na mochila é menor** do que destinarmos esse espaço ao Sonar e ao Colete Salva-Vidas.

:::
???

Em ambos os casos, é relevante conseguir identificar esse "valer a pena", o que pode ser resumido ao **valor da mochila ser maior**. Porém, para saber isso, precisamos quantificar, entre outras coisas, o valor contido na mochila.

Vamos nomear as variáveis do nosso problema:

* **Z** é a função valor da mochila;
* **n** é o número total de itens disponíveis;
* Para cada i itens (i é cumulativo, não confundir) (onde i= 1, 2, …, n), temos:
    * $v(i)$: o valor do i-ésimo item.
    * $w(i)$: o peso do i-ésimo.
* **C** é a capacidade máxima da mochila (ou seja, o peso máximo que ela suporta).

??? Atividade
**Z**, a função valor da mochila, depende de quais variáveis?

::: Gabarito
**Z** é influenciado pelo **valor dos itens** $v(i)$, pois quanto mais itens de valor, mais ela vale.

Também é influenciado pela **capacidade**, pois quanto maior sua capacidade mais itens valiosos podem ser adicionados.

:::
???

Desta forma, podemos chamar de $Z(i, C)$ a função valor da mochila para i itens e capacidade atual C. Assim, temos dois casos: o caso em que o item é colocado na mochila e o caso em que o item não é colocado na mochila.

**Caso 1: O item é colocado na mochila**

Neste caso a mochila passa a ser mais valiosa à medida que o item é valioso. Além disso, sua capacidade reduz na medida do peso do item.

??? Atividade
Escreva a fórmula do valor da mochila para o caso em que o item é adicionado.

**Dica:** Lembre-se que adicionar um item de determinado peso na mochila é equivalente a "trocar" a mochila por outra de capacidade reduzida.

::: Gabarito
Quando um item de peso $w(i)$ e valor $v(i)$ é adicionado na mochila...

* A mochila diminui sua capacidade em $w(i)$ (é "trocada" por uma mochila de capacidade $C-w(i)$).
* A mochila aumenta seu valor em $v(i)$.

Assim, temos que:

$Z(i, C) = v(i) + Z(i-1, C-w(i))$
:::
???

**Caso 2:** O item não é colocado na mochila.

Neste caso, o valor da mochila para i itens é o mesmo valor que para i-1 itens, já que o i-ésimo item não foi adicionado.

$Z(i, C) = Z(i-1, C)$



Agora, temos uma maneira de calcular se vale a pena ou não adicionar um item à mochila:

**O item será adicionado quando a função valor da mochila for superior ao caso em que o item não é adicionado na mochila.**

Organizando os dados
---------

Podemos começar organizando, numa tabela, os valores das somas que nós já fizemos anteriormente.

| Itens     | Peso Total     | Valor Total   | Colocar   |
|----------|----------|----------|----------|
| Fogareiro Portátil        | 7 kg        | R$ 150,00        | Sim   |
| Fogareiro Portátil + Sonar        | 12 kg        | R$ 280,00        | Não   |
| Fogareiro Portátil + Prataria        | 10 kg        | R$ 250,00        | Sim   |
| Sonar + Prataria + Colete Salva-Vidas        | 10 kg        | R$ 290,00        | Sim   |

Porém, desta maneira, ainda estaríamos considerando as combinações feitas por inteiro, e não de maneira sequencial, como estabelecemos anteriormente.

O que faremos, então, é montar uma tabela em que possamos armazenar os valores de $Z(i, C)$, pois ela é capaz de contabilizar a sequencialidade. Ainda, reservaremos as linhas para contagem dos itens e as colunas para contagem da capacidade da mochila. Um exemplo pode ser ilustrado a seguir:

| Capacidade     | 0     | 1   | 2   |
| Itens     |
|----------|----------|----------|----------|
| Nenhum item        | -        | -        | -   |
| Item 1        | -        | -        | -   |
| Item 1, Item 2        | -        | -        | -   |
| Item 1, Item 2, Item 3        | -       | -        | -   |

Então, por exemplo, o valor da mochila quando há um item e a capacidade a mochila é 1 é representado pela célula de linha "Item 1" e coluna "1".

??? Atividade
O que acontece quando não há nenhum item? E quando a capacidade da mochila é zero? Como fica a tabela considerando essas informações?

::: Gabarito
Em ambos os casos o valor da mochila será zero. Portanto:

| Capacidade     | 0     | 1   | 2   |
| Itens     ||||
|----------|----------|----------|----------|
| Nenhum item        | 0        | 0        | 0   |
| Item 1        | 0        | -        | -   |
| Item 1, Item 2        | 0        | -        | -   |
| Item 1, Item 2, Item 3        | 0       | -        | -   |

:::
???

Vamos "navegar" pelo nosso problema. Começaremos com a mochila de capacidade **10kg**, ainda vazia.

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0

Vamos adicionar o Sonar, que pesa 5kg e vale R$ 100,00.

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|
| Sonar        |||||||||||

??? Atividade
O que acontece quando a capacidade da mochila é menor que 5kg? Preencha a tabela.

::: Gabarito
O item não pode ser colocado na mochila, portanto:

$Z(1, C) = Z(0, C) = 0$

Assim:

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|
| Sonar        |0|0|0|0|0|||||||
:::
???

O Sonar é inserido na mochila quando a capacidade a mochila é 5kg. A partir de então, é intuitivo pensar que a mochila terá seu valor.

Se desejarmos, também podemos fazer o cálculo a partir da função $Z(i, C)$ no caso em que o elemento é inserido.

$v(1) = 130$

$Z(1, 10) = v(1) + Z(0, 10-w(1)) = 130 + Z(0, 5) = 130$

Assim, nossa tabela fica da seguinte maneira:

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|
| Sonar        |0|0|0|0|0|130|130|130|130|130|130|

Vamos continuar, adicionando a Prataria, que pesa 3kg e vale 100 R$.

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|
| Sonar        |0|0|0|0|0|130|130|130|130|130|130|
| Prataria        |0|0|0|100|100|||||||

Sabemos que enquanto a capacidade é menor que 3kg, não precisamos nos preocupar, pois nenhum item será adicionado até então. Além disso, a partir de 3kg a Prataria pode ser adicionada, mas a partir de 5kg tanto a Prataria quanto o Sonar podem ser adicionados. Como escolher?

??? Atividade
Qual o valor da mochila quando o peso é entre 5kg e 8kg? E de 8kg a 10kg?

::: Gabarito
Quando 5kg <= C <= 8kg, não podemos adicionar o Sonar e a Prataria ao mesmo tempo, então é preciso ponderar.

Para isso, vamos manter a ideia de analisar item por item, e, nesse caso, estamos analisando a Prataria, então temos dois casos:

Caso 1: Adicionar a Prataria

Vamos supor C = 7kg;

$Z(2, 7) = v(2) + Z(1, 7-3) = v(2) + Z(1, 4)$

**OBS:** Note que Z(1, 4) não é uma célula adjacente. Como estamos subtraindo o peso, podemos ter que consultar qualquer célula da tabela!

$Z(2, 7) = 100 + 0 = 100$

Caso 2: Não adicionar a Prataria

$Z(2, C) = Z(1, C) = 150$

Incrível, não? Sequer precisamos considerar diretamente o Sonar, mas usando a função e consultando a tabela seu valor aparece naturalmente.

Acima de C = 8kg, podemos adicionar os dois itens simultaneamente, portanto:

Assim:

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|
| Sonar        |0|0|0|0|0|130|130|130|130|130|130|
| Prataria        |0|0|0|100|100|130|130|130|230|230|230|
:::
???

Podemos fazer isso para todos os itens. Desta forma, nossa tabela ficará assim:

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|
| Sonar        |0|0|0|0|0|130|130|130|130|130|130|
| Prataria        |0|0|0|100|100|130|130|130|230|230|230|
| Colete Salva-Vidas        |0|0|60|100|100|160|160|160|230|230|290|
| Fogareiro Portátil        |0|0|60|100|100|160|160|160|230|230|290|
| Rádio        |0|0|60|100|100|160|160|160|230|230|290|

Agora que você está começando a entender, podemos partir para o nosso algoritmo.

Algoritmo Dinâmico
----
Para otimizar a solução e reduzir o número de recomputações, usamos a Programação Dinâmica. Aqui a ideia é ter que evitar fazer cálculos excessivos toda vez em que um item for ou não escolhido, dessa forma melhorando a complexidade, e para isso utilizar uma tabela que armazena os valores máximos já calculados.

Vamos criar o algoritmo da nossa [[mochilaDinamica]]. Nesse algoritmo, usamos uma tabela [[tb]] de tamanho [[(n+1) x (capacidade+1)]], onde cada posição $tb[i][c]$ armazena o valor máximo que conseguimos usando os itens até o item [[i]] e com capacidade [[c]].

| Item | Peso | Valor |
|----------|----------|----------|
|         |         ||

```c
int mochilaDinamica(int capacidade, int pesos[], int valores[], int n) {
    int tb[n + 1][capacidade + 1];
}
```
Inicialmente, precisamos percorrer a tabela para preenchê-la, e essa parte é bastante simples e se baseia nas próprias dimensões da tabela:

```c
int mochilaDinamica(int capacidade, int pesos[], int valores[], int n) {
    int tb[n + 1][capacidade + 1];

    for (int i = 0; i <= n; i++) {
        for (int c = 0; c <= capacidade; c++) {
        }
    }
}
```


??? Exercício: Caso base
O que acontece com a tabela para o caso em que não há itens?


::: Gabarito
**Começamos preenchendo a tabela com 0**

Muito bem! A ideia é por aí mesmo.

Agora pense: **O que acontece com a tabela para o caso em que a capacidade da mochila for zero?**

::: Gabarito
Também preenchemos a tabela com 0!
::: 

???

Como bservado, a mochila não ter itens ou a mochila simplesmente não tiver capacidade (imagine que rasgada!!) simplesmente preenchemos como vazia. Dessa forma, passado o caso base, nosso algoritmo vai ganhando corpo:

```c
int mochilaDinamica(int capacidade, int pesos[], int valores[], int n) {
    int tb[n + 1][capacidade + 1];

    for (int i = 0; i <= n; i++) {
        for (int c = 0; c <= capacidade; c++) {
            if (i == 0 || c == 0) {
                tb[i][c] = 0;
            }
        }
    }
}
```

Após a inicialização, percorremos a tabela preenchendo o valor máximo possível para cada item e capacidade. Para cada item i e capacidade c, temos as opções:

1. Se o peso do item é menor ou igual à capacidade ([[pesos[i - 1] <= c]]);
??? Atividade do caso 1
Para este caso em que o item é menor que a capacidade, podemos simplesmente incluir ele?

**DICA:** Pense o que acontece com o peso da mochila depois que incluímos um item, se colocamos ou se não.
::: Gabarito
Temos que verificar se qual será o valor e o peso final se incluímos o item, porque se ultrapassar a capacidade, não nos serve. Ou seja: o valor em $tb[i][c]$ será o maior entre:

1) $tb[i - 1][c - pesos[i - 1]] + valores[i - 1]$ — valor ao incluir o item i.

2) $tb[i - 1][c]$ — valor ao ignorar o item i.
:::
???
2. Se o peso do item é maior que a capacidade.

??? Atividade do caso 2
Para este caso em que o item é maior que a capacidade, o que fazemos com o item?
::: Gabarito
O item não pode ser incluído e $tb[i][c]$ será apenas $tb[i - 1][c]$.
:::
???

Dessa forma, baseado nessas duas opções nosso algoritmo fica:

``` c
else if (pesos[i - 1] <= c) {
    tb[i][c] = (valores[i - 1] + tb[i - 1][c - pesos[i - 1]]) > tb[i - 1][c] ? 
                (valores[i - 1] + tb[i - 1][c - pesos[i - 1]]) : tb[i - 1][c];
} else {
    tb[i][c] = tb[i - 1][c];
}
```

E por fim, Após preencher toda a tabela tb, o valor máximo que pode ser obtido com n itens e capacidade capacidade estará na última célula, $tb[n][capacidade]$.

**E assim está pronto nosso algoritmo da [[mochilaDinamica]].**

```c
int mochilaDinamica(int capacidade, int pesos[], int valores[], int n) {
    int tb[n + 1][capacidade + 1];

    for (int i = 0; i <= n; i++) {
        for (int c = 0; c <= capacidade; c++) {
            if (i == 0 || c == 0) {
                tb[i][c] = 0;
            }
            else if (pesos[i - 1] <= c) {
                tb[i][c] = (valores[i - 1] + tb[i - 1][c - pesos[i - 1]]) > tb[i - 1][c] ? 
                           (valores[i - 1] + tb[i - 1][c - pesos[i - 1]]) : tb[i - 1][c];
            } else {
                tb[i][c] = tb[i - 1][c];
            }
        }
    }

    return tb[n][capacidade];
}
```

Complexidade
----

Agora vamos discutir um pouco sobre a complexidade do algoritmo.

??? Exercício
Pense por um instante, a complexidade desse algoritmo depende de qual característica da tabela?

**DICA:** Pense na tabela criada e como ela é percorrida no algoritmo.

::: Gabarito
A complexidade depende do tamanho da tabela, ou seja, de $n \times C$.
:::
???

A complexidade da abordagem dinâmica depende das próprias dimensões, neste caso dos números de itens e da capacidade da mochila. Dessa forma a complexidade é $O(n \times C)$, sendo:

* $n$ é o número de itens;
* $C$ é a capacidade da mochila. 

Apesar de depender da capacidade da mochila essa abordagem é consideravelmente mais eficiente outros métodos.

**Comparação entre as soluções**

| Método     | Vantagens     | Desvantagens    |
|----------|----------|----------|
| Recursiva        | Fácil de entender e implementar para casos pequenos        | Alta complexidade, muita recomputação        |
| Programação Dinâmica        | Evita recomputação, eficiente para grandes valores de $C$        | Maior uso de memória devido à tabela        |

??? DESAFIO de implementação
Agora pensando na implementação em código, escreva uma versão do algoritmo de forma recursiva.
::: Gabarito
``` c
int mochilaRecursiva(int capacidade, int pesos[], int valores[], int n, int i) {
    if (i == n || capacidade == 0) {
        return 0;
    }

    if (pesos[i] > capacidade) {
        return mochilaRecursiva(capacidade, pesos, valores, n, i + 1);
    }

    int incluirItem = valores[i] + mochilaRecursiva(capacidade - pesos[i], pesos, valores, n, i + 1);
    int naoIncluirItem = mochilaRecursiva(capacidade, pesos, valores, n, i + 1);

    return incluirItem > naoIncluirItem ? incluirItem : naoIncluirItem;
}
```
:::
???
