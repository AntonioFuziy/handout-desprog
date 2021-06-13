Shell Sort
======

**Alunos:** Antonio Fuziy e André Tavernaro

---

Uma visão sobre aplicações do Shell Sort
---------

Antes de tudo, vamos introduzir qual o objetivo e porque é interessante utilizar esse tal de shell sort.

Bom, primeiro então, o shell sort é um algoritmo de ordenação, assim como vimos na [Aula 7](https://ensino.hashi.pro.br/desprog/aula7/index.html) e [Aula 8](https://ensino.hashi.pro.br/desprog/aula8/index.html), e seu funcionamento é semelhante ao do **insertion sort**.

Mas por que utilizar o **Shell Sort** e não qualquer um dos outros cinco algoritmos de ordenação?

Bem, o Shell Sort primeiramente tem uma implementação simples e requer pouco código, além disso ele é rapido para arquivos de tamanho moderado, além disso, na prática, o shell sort obtem melhores resultados que o insertion sort puro. A prova de que ele é um bom algoritmo, é o fato de que ele é utilizado em am algumas bibliotecas, como **uClibc** e **bzip2**, a primeira para sistemas embarcados e dispositivos mobile, enquanto a segunda para compressão de pastas, respectivamente.

Relembrando Insertion Sort
---------

Antes de irmos direto ao shell sort, é interessante revisar o conceito de insertion sort.

Se você não se lembra, a ideia do insertion sort que vimos em aula faz relação a uma situação em que você recebe uma mão de cartas de baralho e toda vez que você pega uma carta na mão, você a insere na posição correta com a finaliade de deixar a sequência de cartas ordenadas.

Dessa forma, temos o exemplo abaixo que deixa mais claro a ideia do insertion sort.

??? Exemplo

Lembre-se que a representação da carta atual na sua mão foi representada por ```c temp``` na animação.

Para o primeiro exemplo, o vetor que você recebe é este:

![](insertion_example.png)

Vamos, te ajudar com o começo, para a primeira iteração, a carta que você pega primeiro é a que está no índice 1 do vetor (lembrando que o primeiro índice é 0) ```c temp = 0```, dessa forma você olha para a carta da posição anterior e ordena as duas de forma crescente (essa foi a forma que utilizamos em aula, mas não seria trivial colocar em ordem decrescente).

;first_insertion

Após a primeira iteração, pega-se a próxima carta (terceira, índice 2) e olha-se para as duas cartas das posições anteriores (índice 0 e 1) e ordena-se do mesmo jeito que na iteração anterior.

;second_insertion

Agora sim, lembramos como funciona o insertion sort, para finalizar, olhe o exemplo da ordenação do vetor inteiro:

;insertion

??? 

Primeira Ideia
---------

Pronto, agora que lembramos a ideia do insertion sort, vamos dar uma introduzida ao shell sort utilizando a ideia já vista do insertion sort.

**Mas porque o shell sort obtem melhores resultados que o insertion sort?**

Para entender isso vamos olhar o vetor abaixo

![](enunciadoInsertion.png)

Se fossemos ordenar esse vetor utilizando o insertion sort tradicional, o código teria que levar o 5 até posição final casa por casa, e depois levar o 1 até a posição inicial casa por casa, o que não é nada prático. De modo geral, o insertion sort tem desempenho ruim nos casos em que é necessário mover um número para um casa muito longe.

??? Atividade

**Mas e se nós fizessemos um insertion sort somente entre as posições 1, 3 e 5?**

![](gapInsertion.png)


::: Gabarito

Aplicando o insertion sort, teremos o subvetor ordenado da seguinte forma:

![](atividade1_01.png)

Após a ordenação colocamos o subvetor no seu devido lugar, e então temos o vetor ordenado como queríamos.

![](atividade1_02.png)

:::
???

Nesse caso tanto o 5 como o 1, teriam que andar menos casas, o que agilizaria muito a ordenação. Esse conceito é a base do shell sort, que nada mais é que um insertion sort aprimorado, e esse "upgrade" é justamente o conceito de "**gap**". De forma resumida, o shell sort é capaz de mover elementos por distancias maiores de uma vez, o que não é possível no insertion sort, que move elementos de casa em casa.

Gaps
---------

O termo "**gap**" significa "Vão", e é exatamente isso que acontece no nosso vetor. De forma resumida, para cada iteração do código, é definido um valor para "**gap**", desse modo, são organizados subvetores imaginários que são os termos do vetor original intercalados por esse valor definido. Talvez fique mais claro com o exemplo abaixo:


```c
    {4, 2, 8, 5, 9, 3}
```

![](exemplos-gap/exemplo-gap-enunciado.png)

Simulando o vetor inicial com Gap = 2 , temos os 2 subvetores imaginários abaixo:

```c
     {4, 8, 9}
     {2, 5, 3}
```

![](exemplos-gap/exemplo-gap2.png)

Vamos praticar um pouco com a atividade abaixo:

??? Atividade

Tendo o vetor abaixo, pense como seriam os subvetores para um Gap = 3.

```c
    {9, 5, 2, 1, 8, 4}
```

![](exemplos-gap/exercicio-gap-enunciado.png)

::: Gabarito
Teriamos 3 Subvetores imaginarios, cada um deles com 2 termos:
```c
    {9, 1}
    {5, 8}
    {2, 4}
```

![](exemplos-gap/exercicio-gap-gabarito.png)
:::

???


Implementação
---------

De forma resumida, na primeira iteração é definido um valor de gap e o algoritmo faz essa divisão de subvetores imaginários de acordo com o valor, e os ordena de forma muito semelhante ao insertion sort. 

Desse modo, na próxima iteração, um novo valor de gap é definido e o processo se repete. Vale ressaltar que os gaps maiores não são garantia que o vetor fique todo ordenado, o único jeito de garantir é diminuir o gap até que o valor seja 1, uma ordenação com gap = 1 nada mais é que o insertion sort tradicional, no entanto ele terá muito menos trabalho pois o vetor já vai estar pelo menos quase ordenado, tendo assim no final, um vetor totalmente ordenado.

Mas como são definidos os valores dos **gaps** ?

Basicamente, diversos matemáticos definiram inúmeras listas com valores de **gaps**, desse modo, a cada iteração, o algoritmo usa um valor dessa lista, começando com o primeiro valor e terminando sempre no ultimo valor, que é sempre 1.

Mas então por que existem diversas listas e não só uma?

Isso se deve ao fato de que não foi descoberto uma sequência de **gaps** que seja a melhor em todas as situações, cada uma das sequências pode servir muito bem para um caso específico e muito mal para outro. O que faz com que este algoritmo não tenha uma complexidade geral, e sim uma complexidade para cada sequência de **gaps**.

Dessa forma, utilizando uma implementação do código do shell sort com um tipo de gap talvez deixe mais claro 

```txt
para todo gap maior que 0, divida o gap ao meio
    para todo gap menor que n, vai para a proxima posicao
        guardar o valor do termo atual do vetor
        
        localizar a posição que o elemento será inserido
        
        inserir o elemento guardado anteriormente na posição localizada
```
Entrando mais a fundo, a primeira linha do pseudocódigo retrata po loop mais externo o qual representa a extração do subvetor a partir do vetor recebido, ao passo em que o loop mais interno retrada o **insertion sort**, ordenando o subvetor extraído do loop externo.

!!! Aviso
Vale ressaltar, que utilizamos para esse exemplo de pseudocódigo utilizamos um tipo de gap, dividimos o gap sempre na metade, mas como citamos anteriormente, existem vários tipos de gaps para ordenações de vetores, sendo uma sequência de gaps mais adequada para cada vetor a ser ordenado.
!!!

??? Exemplo

Abaixo temos um exemplo do shell sort em prática, a sequência de gaps utilizada é de [4,3,2,1]

;diagrama


!!! Aviso
Vale lembrar que no exemplo, os subvetores são ordenados de uma vez, no entanto isso não acontece na realidade, a ordenação desses subvetores imaginários é feita de modo semelhante ao insertion sort, ou seja, os números são movidos um de cada vez e não de uma vez só!
!!!
???
---

??? Atividade

Tendo o vetor abaixo, pense em como ele ficaria antes de ir para o próximo gap, ou seja, no final de cada iteração. **A sequência de gaps utilizada ainda é a mesma do exemplo anterior.** 

Vamos fazer primeiro para o **gap 4**.

![](ex-shell.png)

::: Gabarito
gap -> 4

![](atividade2_2.png)
:::
???

??? Atividade
Vamos agora fazer para o **gap 3** e assim por diante.

::: Gabarito
gap -> 3

![](atividade2_3.png)
:::
???

??? Atividade
**gap 2**

::: Gabarito
gap -> 2

![](atividade2_4.png)
:::
???

??? Atividade
**gap 1**

**Dica: Nessa última iteração a ordenação é igual a do insertion sort**

::: Gabarito
gap -> 1

![](atividade2_5.png)
:::
???
## Complexidade

Vamos agora dar uma olhada em como funciona a complexidade do shell sort. Bom, basicamente o shell sort não apresenta uma complexidade fixa, uma vez que a sua complexidade varia conforme a sequência escolhida para os **gaps**, se você voltar para o exemplo do algoritmo do shell sort implementado em C, é possível notar que a sequência de gaps é $\frac{n}{2}$.
e a complexidade eh quadratica baseada no tamanho do vetor.

**Sequência original de Shell (Pior caso)**

- Sequência de gaps escolhida:

$$\frac{n}{2^k}$$

- Para todo valor de k:

$$k = {1, 2, 3, 4, 5 ...}$$

??? Atividade

Substituindo os valores de k na sequência de gaps como ficará a sequência resultante?


::: Gabarito
$$\frac{n}{2} + \frac{n}{4} + \frac{n}{8} + \frac{n}{16}...$$
:::
???

- Mas, lembre-se, como todo subvetor imaginário do shell sort é ordenado pelo **insertion sort**, e a complexidade de todo o insertion sort é quadrática (
$O(n^2)$), o que ocorre é que a sequência de gaps somada, dependerá dessa ordenação. Assim, a soma de todos os gaps fica:

$$\frac{n^2}{2} + \frac{n^2}{4} + \frac{n^2}{8} + \frac{n^2}{16}...$$

- Deixando o $n^2$ em evidência, temos que:

$$n^2 . (\frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \frac{1}{16}...)$$

??? Atividade

Olhando para a sequência que temos até agora já conseguimos visualizar qual será o resultado da complexidade do shell sort, veja se você consegue perceber logo de cara.

**Dica, olhe para o $n^2$**

::: Gabarito

- Abrindo-se a sequência em uma PG de razão $\frac{1}{2}$, primeiro termo igual a $\frac{1}{2}$, temos que a soma da PG fica:

$$n^2 . (\frac{\frac{1}{2}}{1 - \frac{1}{2}})$$

- Resolvendo-se a soma de PG tem-se então que:

$$n^2 + 1$$

- Por fim, como temos a sequência dependente apenas de $n^2$, podemos concluir que a complexidade da sequência original de Shell Sort é:

$${O(n^2)}$$

:::
???

!!! Aviso
Na prática, a sequência original de shell mostra o pior caso de complexidade do algoritmo, porém isso não acontece para todos os casos, pois como toda a ordenação por gaps trabalha com vetores "quase ordenados", é importante ressaltar que há estratégias diferentes em que a sequência de gaps escolhida deixa o algoritmo mais performático que o insertion sort na prática.
!!!

Com o passar dos anos, além da sequência original de shell, algumas outras sequências de gaps mais eficientes que a original foram encontradas, como a sequência de Knuth, Pratt, Hibbard, Sedgewick e outras. Assim, até hoje vários pesquisadores buscam a melhor sequência de gaps para o shell sort.

**Abaixo temos algumas dessas sequências de gaps e suas devidas complexidades, apenas para mostrar que a complexidade varia conforme a sequência escolhida:**

---

**Sequência de Knuth**

- Sequência de gaps escolhida:

$$\frac{3^k - 1}{2}$$

- Para todo valor de k:

$$k = {1, 2, 3, 4, 5 ...}$$

- Substituindo os valores de k na sequência de gaps, ficamos com:

$$ 1 + 4 + 13 + 40...+ \frac{3^k - 1}{2}$$

- Por fim a complexidade acaba em:

$${O(n^\frac{3}{2})}$$

---

**Sequência de Pratt**

- Sequência de gaps escolhida:

$${2^p . 3^q}$$

- Para todo valor de p e q, sendo:

$$p = {0, 1, 2, 3, 4, 5 ...}$$
$$q = {0, 1, 2, 3, 4, 5 ...}$$

- Substituindo os valores de $p$ e $q$ na sequência de gaps, ficamos com:

$$ {2^0 . 3^0} + {2^1 . 3^0} + {2^0 . 3^1} + {2^1 . 3^1} + {2^2 . 3^1} + {2^1 . 3^2} + {2^2 . 3^2}...$$

- Por fim a complexidade acaba em:

$${O(n.log^2{n})}$$

??? Desafio

Faça um esboço dos três gráficos das complexidades citadas anteriormente de acordo com os tamanhos de vetor e coloque as três em ordem crescente de complexidade.

::: Gabarito
![](graphics.png)

Como temos a sequência original de shell sendo quadrática, automaticamente entre as três ela é a maior de todas, ao passo em que a segunda maior é a de Pratt e por fim o menor de todos é a com logaritmo, ao observarmos o gráfico.

$$ O(n.log^2{n}) < O(n^\frac{3}{2}) < O(n^2)$$
:::
???

??? Dedução final

Para finalizar, queremos apenas confirmar se você entendeu a ideia do porquê utilizar um shell sort em alguns casos e não o insertion sort. Mas afinal, porque você acha que seria mais viável utilizar o shell sort ao invés do insertion?

::: Gabarito

A ideia geral é que o shell sort seja utilizado para vetores mais desordenados, utilizando os gaps para ordenar os elementos que estão mais distantes um do outro, dessa forma, **é mais prático trabalhar com gaps pequenos quando o vetor está quase ordenado**, de forma que quando o gap da ordenação é 1, utilizar o shell sort é a mesma coisa de utilizar o insertion sort puro para ordenar o vetor.

:::
???