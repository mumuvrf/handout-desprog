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




Algoritmo recursivo
-----

O problema da Mochila Binária é um clássico de otimização, onde precisamos maximizar o valor total dos itens na mochila sem ultrapassar sua capacidade. Para essa tarefa podemos usar duas abordagens: uma recursiva e por programação dinâmica, que é mais eficiente.

Nosso objetivo é entender como o algoritmo funciona, construir o código em partes e desenvolver intuição sobre as vantagens e limitações de cada método.

A primeira forma de resolver o problema da mochila é usando a **abordagem recursiva**. Esse método funciona bem para entender a essência do problema, mas pode ser lento quando o número de itens ou a capacidade é muito grande.

Na recursiva, a cada item, o nosso amigo Anderson tem duas opções:

1. Não incluir o item no saco.
2. Incluir o item no saco, desde que o peso do item seja menor ou igual à capacidade restante.

Para fazer isso em C, vamos criar uma função chamada [[mochilaRecursiva]]. Vamos começar a definir a estrutura básica dessa função. Essa função vai receber como parâmetros:

* A capacidade restante do saco (capacidade).
* Um vetor de pesos (pesos) e um vetor de valores (valores) para os itens.
* O número total de itens (n) e o índice do item atual (i).

Primeiro, definimos nossa função com esses parâmetros e começamos com o caso base, que indica quando a recursão deve parar.

``` c
int mochilaRecursiva(int capacidade, int pesos[], int valores[], int n, int i) {
}
```

??? Exercício: Identificando o caso base
O que acontece se já consideramos todos os itens ou se a capacidade for zero? 

**DICA:** Pense numa mochila vazia ou numa situação em que todos os itens já foram verificados.

::: Gabarito
``` c
int mochilaRecursiva(int capacidade, int pesos[], int valores[], int n, int i) {
    if (i == n || capacidade == 0) {
        return 0;
    }
}
```

No código acima, começamos verificando se já analisamos todos os itens ou se a capacidade do saco é zero. Em ambos os casos, o valor máximo que podemos obter é zero, então retornamos [[0]].
:::
???

Após o caso base, a próxima verificação é saber se o item atual pode ser incluído no saco. Isso depende de o peso do item ser menor ou igual à capacidade restante do saco.

``` c
if (pesos[i] > capacidade) {
    return mochilaRecursiva(capacidade, pesos, valores, n, i + 1);
}
```

Se o item pode ser incluído, temos duas escolhas: incluir o item ou não incluir o item. Para isso, usamos duas chamadas recursivas:

1. Se incluímos o item i, somamos seu valor ao valor máximo obtido considerando os próximos itens e a capacidade restante após incluir o peso desse item:

``` c
int incluirItem = valores[i] + mochilaRecursiva(capacidade - pesos[i], pesos, valores, n, i + 1);
```

2. Se não incluímos o item i, chamamos [[mochilaRecursiva]] para o próximo item, com a capacidade inalterada.

```c
int naoIncluirItem = mochilaRecursiva(capacidade, pesos, valores, n, i + 1);
```
3. Por fim, devolvemos o valor máximo entre incluirItem e naoIncluirItem para garantir que estamos maximizando o valor da mochila.

```c
return incluirItem > naoIncluirItem ? incluirItem : naoIncluirItem;
```

Dessa forma, está pronta a função recursiva do problema da mochila e o nosso amigo Anderson pode finalmente ajudar Durão Barba Ruiva a encher a sacola com os itens mais valiosos.

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

No entanto, {red}(nem tudo são flores...)

??? Exercício
O algoritmo recursivo tem um sério problema de alta complexidade computacional. Reflita: O que faz esse algoritmo ter esse tipo de comportamento?

**DICA:** Pense no momento em que ocorre a chamada recursiva.

::: Gabarito
A solução recursiva tem alta complexidade exponencial ($2^n$), pois recalcula subproblemas várias vezes. Por exemplo, ao calcular o valor máximo com e sem o item 1, recalculamos subproblemas para cada um dos outros itens. É aí que entra a Programação Dinâmica, que evita essa recomputação.
:::
???

Algoritmo Dinâmico
----
Para otimizar a solução e reduzir o número de recomputações, usamos a Programação Dinâmica. Aqui a ideia é ter que evitar fazer cálculos excessivos toda vez em que um item for ou não escolhido, dessa forma melhorando a complexidade, e para isso utilizar uma tabela que armazena os valores máximos já calculados.

Vamos criar uma função chamada [[mochilaDinamica]]. Nesta função, usamos uma tabela [[tb]] de tamanho [[(n+1) x (capacidade+1)]], onde cada posição $tb[i][c]$ armazena o valor máximo que conseguimos usando os itens até o item [[i]] e com capacidade [[c]].

```c
int mochilaDinamica(int capacidade, int pesos[], int valores[], int n) {
    int tb[n + 1][capacidade + 1];
}
```

??? Exercício: Caso base
O que acontece com a tabela para o caso em que não há itens ou simplesmente a capacidade da mochila for zero?

**DICA:** Pense no algoritmo recursivo quando o mesmo problema foi abordado.

::: Gabarito
**Começamos preenchendo a tabela com 0 para a capacidade de 0 e para 0 itens.**

Muito bem! A ideia é por aí mesmo.

Agora pense: como que essa tabela vai ser preenchida? Ou seja, de que forma percorrer os índices da tabela para preencher com zeros?

::: Gabarito
```c
for (int i = 0; i <= n; i++) {
    for (int c = 0; c <= capacidade; c++) {
        if (i == 0 || c == 0) {
            tb[i][c] = 0;
        }
    }
}
```
:::
???

Após a inicialização, percorremos a tabela preenchendo o valor máximo possível para cada item e capacidade. Para cada item i e capacidade c, temos as opções:

1. Se o peso do item é menor ou igual à capacidade ([[pesos[i - 1] <= c]]), podemos incluir o item e o valor em $tb[i][c]$ será o maior entre:

    * $tb[i - 1][c - pesos[i - 1]] + valores[i - 1]$ — valor ao incluir o item i.
    * $tb[i - 1][c]$ — valor ao ignorar o item i.

??? Exercício
Como uma forma de exercitar o raciocínio e fixar o aprendizado, implemente as condições descritas acima ao código já construído até então.

**DICA:** Construa o código seguindo exatamente a descrição do caso acima.

::: Gabarito
```c
if (pesos[i - 1] <= c) {
    if ((valores[i - 1] + tb[i - 1][c - pesos[i - 1]]) > tb[i - 1][c]) {
        tb[i][c] = valores[i - 1] + tb[i - 1][c - pesos[i - 1]];
    } else {
        tb[i][c] = tb[i - 1][c];
    }
}
```

Uma outra forma de fazer (tem exatamente o mesmo propósito):
```c
if (pesos[i - 1] <= c) {
    tb[i][c] = (valores[i - 1] + tb[i - 1][c - pesos[i - 1]]) > tb[i - 1][c] ? 
                (valores[i - 1] + tb[i - 1][c - pesos[i - 1]]) : tb[i - 1][c];
}
```
:::
???

2. Se o peso do item é maior que a capacidade, o item não pode ser incluído e $tb[i][c]$ será apenas $tb[i - 1][c]$.

??? Exercício
Seguindo a ideia do exercício anterior, implemente a condição acima descrita ao código já construído até então.

**DICA:** Construa o código seguindo exatamente a descrição do caso acima.

::: Gabarito
```c
else if (pesos[i - 1] > c) {
    tb[i][c] = tb[i - 1][c];
}
```

Ou de um jeito melhor:

```c
else {
    tb[i][c] = tb[i - 1][c];
}
```
:::
???

E por fim, Após preencher toda a tabela tb, o valor máximo que pode ser obtido com n itens e capacidade capacidade estará na última célula, $tb[n][capacidade]$.

```c
return tb[n][capacidade];
```

E assim está pronto nossa função [[mochilaDinamica]]: 

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

??? Exercício
Agora pensando na complexidade desse algoritmo, qual seria?

**DICA:** Pense na tabela criada e como ela é percorrida no algoritmo.

::: Gabarito
A complexidade da abordagem dinâmica é $O(n \times C)$, onde $n$ é o número de itens e $C$ é a capacidade da mochila. Apesar de depender da capacidade da mochila essa abordagem é consideravelmente mais eficiente que a recursiva.
:::
???

**Comparação entre as soluções**

| Método     | Vantagens     | Desvantagens    |
|----------|----------|----------|
| Recursiva        | Fácil de entender e implementar para casos pequenos        | Alta complexidade, muita recomputação        |
| Programação Dinâmica        | Evita recomputação, eficiente para grandes valores de $C$        | Maior uso de memória devido à tabela        |