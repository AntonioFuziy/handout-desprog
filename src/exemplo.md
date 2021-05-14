Shell Sort
======

**Alunos:** Antonio Fuziy e André Tavernaro

---

Primeira ideia
---------

Antes de tudo, vamos introduzir qual o objetivo e porque é interessante utilizar esse tal de shell sort.

Bom, primeiro então, o shell sort é um algoritmo de ordenação, assim como vimos na [Aula 7](https://ensino.hashi.pro.br/desprog/aula7/index.html) e [Aula 8](https://ensino.hashi.pro.br/desprog/aula8/index.html), e seu funcionamento é semelhante ao do **insertion sort**. Na prática, atualmente o Shell Sort é utilizado em am algumas bibliotecas, como **uClibc** e **bzip2**, a primeira para sistemas embarcados e dispositivos mobile, enquanto a segunda para compressão de pastas, respectivamente.

Mas por que utilizar o **Shell Sort** e não qualquer um dos outros cinco algoritmos de ordenação?

Bem, o Shell Sort primeiramente tem uma implementação simples e requer pouco código, além disso ele é rapido para arquivos de tamanho moderado. No entanto a característica mais relevante deste algoritmo é que, ao contrário do insertion sort que só move elementos de posição em posição, o shell consegue mover elementos que estão longe um do outro de uma vez só, atrávez de um recurso chamado **gaps**.

Gaps
---------

O termo "**gap**" significa "Vão", e é exatamente isso que acontece no nosso vetor. De forma resumida, para cada iteração do código, é definido um valor para "**gap**", desse modo, são organizados subvetores imaginários que são os termos do vetor original itercalados por esse valor definido. Talvez fique mais claro com o exemplo abaixo:


```c
    {4, 2, 8, 5, 9, 3}
```

![](exemplos-gap/exemplo-gap-enunciado.png)

Simulando o vetor acima com Gap = 3 , temos os 3 subvetores imaginários abaixo:

```c
     {4, 5}
     {2, 9}
     {8, 3}
```

![](exemplos-gap/exemplo-gap3.png)

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

De forma resumida, na primeira iteração é definido um valor de gap e o algoritmo faz essa divisão de subvetores imaginários de acordo com o valor, e os ordena de forma muito semelhante ao insertion sort. Desse modo, na próxima iteração, um novo valor de gap é definido e o processo se repete, até o gap chegar a 1, nesta etapa é feito um insertion sort tradicional, tendo um assim um vetor ordenado como produto final.

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

??? Atividade

Pense um pouco sobre como seria o pseudocódigo anterior em C.

Sim, eu sei que pode ser complicado, mas tente pelo menos pensar um pouco como ficaria, e confira depois com o gabarito para validar seu pensamento.

::: Gabarito
```c
void shell_sort(int v[], int n) {
    // para todo gap maior que 0, divida o gap ao meio
    for (int gap = n/2; gap > 0; gap /= 2) {
        
        // para todo gap menor que n, vai para a proxima posicao
        for (int i = gap; i < n; i += 1) {
            // guardar o valor do termo atual do vetor
            int atual = v[i];
 
            // localizar a posição que o elemento será inserido
            int j;           
            for (j = i; j >= gap && v[j - gap] > atual; j -= gap){
                v[j] = v[j - gap];
            }
             
            // inserir o elemento guardado anteriormente na posição localizada
            v[j] = atual;
        }
    }
}
```

Vale ressaltar, que utilizamos para esse exemplo utilizamos um tipo de gap, dividimos o gap sempre na metade, mas como citamos anteriormente, existem vários tipos de gaps para ordenações de vetores, sendo uma sequência de gaps mais adequada para cada vetor a ser ordenado.
:::
???
??? Exemplo

Abaixo temos um exemplo do shell sort em prática, a sequência de gaps utilizada é de [4,3,2,1]

;diagrama


!!! Aviso
Vale lembrar que no exemplo, os subvetores são ordenados de uma vez, no entanto isso não acontece na realidade, a ordenação desses subvetores imaginários é feita de modo semelhante ao insertion sort, ou seja, os números são movidos um de cada vez e não de uma vez só!
!!!
???
---

??? Atividade

Tendo o vetor abaixo, pense em como ele ficaria antes de ir para o próximo gap, ou seja, no final de cada iteração. **A sequência de gaps utilizadas ainda é a mesma do exemplo anterior.** 

![](ex-shell.png)

::: Gabarito
![](gabarito-ex-shell.png)
:::
???
## Complexidade

Vamos agora dar uma olhada em como funciona a complexidade do shell sort. Bom, basicamente o shell sort não apresenta uma complexidade fixa, uma vez que a sua complexidade varia conforme a sequência escolhida para os **gaps**, se você voltar para o exemplo do algoritmo do shell sort implementado em C, é possível notar que a sequência de gaps é n/2 

a complexidade eh quadratica baseada no tamanho do vetor

**Sequência original de Shell**

- Sequência de gaps escolhida:

$$\frac{n}{2^k}$$

- Para todo valor de k:

$$k = {1, 2, 3, 4, 5 ...}$$

- Substituindo os valores de k na sequência de gaps, ficamos com:

$$\frac{n}{2} + \frac{n}{4} + \frac{n}{8} + \frac{n}{16}...$$

- Mas, lembre-se, como todo subvetor imaginário do shell sort é ordenado pelo **insertion sort**, e a complexidade de todo o insertion sort é quadrática (
$O(n^2)$), o que ocorre é que a sequência de gaps somada, dependerá dessa ordenação. Assim, a soma de todos os gaps fica:

$$\frac{n^2}{2} + \frac{n^2}{4} + \frac{n^2}{8} + \frac{n^2}{16}...$$

- Deixando o $n^2$ em evidência, temos que:

$$n^2 + (\frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \frac{1}{16}...)$$

- Abrindo-se a sequência em uma PG de razão $\frac{1}{2}$, primeiro termo igual a $\frac{1}{2}$, temos que a soma da PG fica:

$$n^2 + (\frac{\frac{1}{2}}{1 - \frac{1}{2}})$$

- Resolvendo-se a soma de PG tem-se então que:

$$n^2 + 1$$

- Por fim, como temos a sequência dependente apenas de $n^2$, podemos concluir que a complexidade da sequência original de Shell Sort é:

$${O(n^2)}$$

**Abaixo temos algumas outras sequências e suas devidas complexidades, apenas para mostrar que a complexidade varia conforme a sequência de gaps escolhida:**

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
