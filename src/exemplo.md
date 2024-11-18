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

Anderson lhe entregou a mochila, que suporta **15 kg**. Você, então, pediu que a tripulação do S.S. Paulina organizasse os itens e lhe trouxessem.

Foi lhe entregue o seguinte:

* **Itens**:
    * **Binóculos**, pesa 1 kg e vale R$ 10,00;
    * **Saco de batatas**, pesa 9 kg e vale R$ 170,00;
    * **Prataria**, pesa 3 kg e vale R$ 100,00;
    * **Sonar**, pesa 5 kg e vale R$ 130,00;
    * **Rádio**, pesa 4 kg e vale R$ 90,00;
    * **Colete salva-vidas**, pesa 2 kg e vale R$ 60,00;
    * **Fogareiro portátil**, pesa 7 kg e vale R$ 150,00.

Vamos pensar por partes. Para encher a mochila, temos que ter um ponto de partida. Esse ponto de partida pode ser o primeiro item!

??? Atividade
Se você pudesse colocar um, e apenas um, item para colocar na mochila. Qual o processo por trás dessa escolha?

::: Gabarito
Podemos pensar "Se o Durão Barba-Ruiva só pudesse sair daqui com um item, qual item o faria feliz?" Nesse caso, importa o item que é mais valioso, e portanto o mais intuitivo é fazer uma varredura pela lista de itens procurando esse item. Assim, chegamos ao **Saco de batatas** que vale R$ 170,00.
:::
???

Colocar um item é fácil, pois há poucas informações a considerar. Assim, pudemos focar em uma única parte do problema: Maximizar o valor total na mochila. Porém, se tivermos que colocar dois ou mais itens, a escolha já pode não ser tão imediata.

??? Atividade
Selecione dois itens para colocar na mochila. Como você chegou a essa combinação? Que novos problemas surgiram?

::: Gabarito
Com dois itens, agora é necessário considerar não somente o peso e valor de cada item, mas a soma deles. Isso traz três novas situações:

- **A soma dos pesos dos itens ser maior que a capacidade da mochila**: Isso é o caso para a combinação Saco de Batatas + Fogareiro Portátil (9 + 7 = 16 kg), o que torna essa combinação inválida.
- **É preciso analisar várias informações de uma vez**: Você pode ter que somar os pesos e valores de vários itens, e ter controle das informações se torna difícil.
- **Pode existir uma outra combinação com mais itens que tenha maior valor**: Com dois itens, podemos maximizar o valor selecionando o Saco de Batatas e o Sonar (9 + 5 = 14 kg < 15 kg), totalizando 170 + 130 = R$ 300,00. Porém, com três itens, poderíamos combinar o Sonar, o Rádio e a Prataria (5 + 4 + 3 = 12 kg < 15 kg), totalizando 130 + 90 + 100 = R$ 320,00.
:::
???

Quando consideramos, então, todos os itens a fim de encontrar uma combinação, temos que ter em mente a possibilidade de existir uma combinação alternativa com um valor maior. Isso nos leva a testar todas as somas de pesos e valores possíveis, o que, sem um método de organização e utilização dessas informações, se torna trabalhoso.

Como, então, podemos otimizar a análise de todas as combinações?

Descrição matemática do problema
----

A primeira coisa que devemos aceitar sobre o problema é que não temos a capacidade de avaliar todos os dados simultaneamente, então precisamos seguir um método sequencial. Desta forma, temos que pensar em **qual item devo adicionar em seguida** ao invés de qual combinação posso fazer de imediato.

Para isso, podemos usar o artifício matemático de pensar que, quando um item é adicionado, isso é equivalente a eu "trocar" a minha mochila por uma outra com capacidade reduzida pelo peso do item. Por exemplo, quando eu coloco o Saco de Batatas que pesa 9 kg na mochila que cabe 15 kg, isso é o mesmo que recomeçar o problema do zero com uma mochila de 6 kg. Desta forma, podemos pensar em cada inserção de maneira quase independente.

Em seguida, é preciso, então, contabilizar pelo valor, afinal queremos a combinação de melhor retorno. Como, para cada item, há duas opções: Colocar ou não colocar o item (por isso, inclusive, esse problema é denominado Problema da Mochila Binária), utilizemos o seguinte critério para decisão acerca de um item: **O item deverá ser colocado na mochila se seu valor for maior que o valor do espaço ocupado por ele. Isto é, vale mais a pena colocar este item do que destinar o mesmo espaço para outros itens.**

Parece intuitivo, não? Porém, como podemos quantificar isso?

Vamos representar os elementos do problema:

* **n** é o número total de objetos disponíveis;
* Para cada objeto i (onde i= 1, 2, …, n), temos:
    * $v(i)$: o valor do objeto i.
    * $w(i)$: o peso do objeto i.
* **C** é a capacidade máxima da mochila (ou seja, o peso máximo que ela suporta).

Vamos chamar de $Z(i, C)$ a função valor da mochila para i itens e capacidade atual C. Assim, temos dois casos: o caso em que o item é colocado na mochila e o caso em que o item não é colocado na mochila.

**Caso 1:** O item é colocado na mochila.
Neste caso, o valor da mochila é o valor do item somado ao valor da mochila quando tem a capacidade reduzida pelo peso do item e há i-1 itens disponíveis.

$Z(i, C) = v(i) + Z(i-1, C-w(i))$

??? Atividade
Após colocarmos o Saco de Batatas (i=1) na mochila, temos que Z(i-1, C-w(i)) = R$ 170,00. Se colocarmos o Sonar (i=2), qual o valor da mochila?

::: Gabarito
O valor do Sonar é R$ 130,00, portanto:

$v(i) = 130$

Logo:

$Z(i, C) = 130 + 170 = 300$
:::
???

**Caso 2:** O item não é colocado na mochila.

Neste caso, o valor da mochila para i itens é o mesmo valor que para i-1 itens, já que o i-ésimo item não foi adicionado.

$Z(i, C) = Z(i-1, C)$

??? Atividade
Ainda na situação em que já colocamos o Saco de Batatas, o que ocorre com o valor da mochila se não colocarmos o Sonar?

::: Gabarito
O valor da mochila não muda

$Z(i, C) = Z(i-1, C) = 170$
:::
???

Desta forma, adicionaremos o item na mochila quando o valor Z obtido no caso 1 for maior que o valor obtido no caso 2, ou seja, quando o item for mais valioso que deixar disponível o espaço equivalente a seu peso.

Agora, temos uma maneira de calcular se vale a pena ou não adicionar um item à mochila. Resta, então, manter controle desses cálculos.

Organizando os dados
---------

Podemos começar organizando, numa tabela, os valores das somas que nós já fizemos anteriormente.

| Itens     | Peso Total     | Valor Total   | Colocar   |
|----------|----------|----------|----------|
| Saco de Batatas        | 9 kg        | R$ 170,00        | Sim   |
| Saco de Batatas + Fogareiro Portátil        | 16 kg        | R$ 320,00        | Não   |
| Saco de Batatas + Sonar        | 14 kg        | R$ 300,00        | Sim   |
| Sonar + Rádio + Prataria        | 13 kg        | R$ 320,00        | Sim   |

Porém, desta maneira, ainda estaríamos considerando as combinações feitas por inteiro, e não de maneira sequencial, como estabelecemos anteriormente.

O que faremos, então, é montar uma tabela em que possamos armazenar os valores da função Z, pois ela é capaz de contabilizar a sequencialidade. Ainda, reservaremos as linhas para contagem dos itens e as colunas para contagem da capacidade da mochila. Um exemplo pode ser ilustrado a seguir:

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

Então, por exemplo, para o caso em que só temos uma mochila de capacidade 15 kg e somente o Saco de Batatas, a tabela ficaria da seguinte forma:

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0
| Saco de Batata        |0|0|0|0|0|0|0|0|0|170|170|170|170|170|170|170

Note que o valor só passa a ser 170 quando a capacidade da mochila é igual ou maior que 9 kg. Isso será útil para quando houver mais de um item. Por exemplo, com o Saco de Batatas e o Sonar:

| Capacidade     |0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15
| Itens     ||||||||||||||||
|----------|----------|----------|----------|
| Nenhum item        |0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0
| Saco de Batata        |0|0|0|0|0|0|0|0|0|170|170|170|170|170|170|170
| Sonar        |0|0|0|0|0|130|130|130|130|170|170|170|170|170|300|300

Apesar de somente o Sonar estar indicado, na linha 2 já se presume a existência de dois itens: o Saco de Batata e o Sonar. Por essa razão, quando o peso atinge 5kg, o valor passa a ser o do Sonar até alcançar 9 kg, quando o valor do Saco de Batata passa a ser maior. Por fim, a partir de 14, os dois itens podem ser colocados juntos na mochila, portanto o valor passa a ser 300.

Podemos fazer isso para todos os itens, mas isso é suficiente para seguirmos para o algoritmo.

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
