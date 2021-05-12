Shell Sort
======

Primeira ideia
---------

Antes de tudo, vamos introduzir qual o objetivo e porque é interessante utilizar esse tal de shell sort.

Bom, primeiro então, o shell sort é um algoritmo de ordenação, assim como vimos na [Aula 7](https://ensino.hashi.pro.br/desprog/aula7/index.html) e [Aula 8](https://ensino.hashi.pro.br/desprog/aula8/index.html), e seu funcionamento é semelhante ao do **insertion sort**. Na prática, atualmente o Shell Sort é utilizado em am algumas bibliotecas, como **uClibc** e **bzip2**, a primeira para sistemas embarcados e dispositivos mobile, enquanto a segunda para compressão de pastas, respectivamente.

Mas por que utilizar o **Shell Sort** e não qualquer um dos outros cinco algoritmos de ordenação?

Bem, o Shell Sort primeiramente tem uma implementação simples e requer pouco código, além disso ele é rapido para arquivos de tamanho moderado. No entanto a característica mais relevante deste algoritmo é que, ao contrário do insertion sort que só move elementos de posição em posição, o shell consegue mover elementos que estão longe um do outro de uma vez só, atrávez de um recurso chamado **gaps**.

Gaps
---------

O termo "**Gap**" significa "Vão", e é exatamente isso que acontece no nosso vetor. De forma resumida, para cada iteração do código, é definido um valor para "**gap**", desse modo, são organizados subvetores imaginários que são os termos do vetor original itercalados por esse valor definido. Talvez fique mais claro com o exemplo abaixo:


```c
    {4, 5, 2, 3, 1}
```
Simulando o vetor acima com Gap = 3 , temos os 3 subvetores imaginários abaixo:

```c
     {4, 3}
     {5, 1}
     {2}
```

Simulando o vetor acima com Gap = 2 , temos os 2 subvetores imaginários abaixo:

```c
     {4,2,1}
     {5,3}
```

Vamos praticar um pouco com a atividade abaixo:

??? Atividade

Tendo o vetor abaixo, pense como seriam os subvetores para um Gap = 3.

```c
    {2, 8, 5, 4, 1, 6}
```

::: Gabarito
Teriamos 3 Subvetores imaginarios, cada um deles com 2 termos:
```c
    {2, 4}
    {8, 1}
    {5, 6}
```
De forma resumida, na primeira iteração é definido um valor de gap e o algoritmo faz essa divisão de subvetores imaginários de acordo com o valor, e os ordena de forma muito semelhante ao insertion sort. Desse modo, na próxima iteração, um novo valor de gap é definido e o processo se repete, até o gap chegar a 1, nesta etapa é feito um insertion sort tradicional, tendo um assim um vetor ordenado como produto final.

Mas como são definidos os valores dos **gaps** ?

Basicamente, diversos matemáticos definiram inúmeras listas com valores de **gaps**, desse modo, a cada iteração, o algoritmo usa um valor dessa lista, começando com o primeiro valor e terminando sempre no ultimo valor, que é sempre 1.

Mas então por que existem diversas listas e não só uma?

Isso se deve ao fato de que não foi descoberto uma sequência de **gaps** que seja a melhor em todas as situações, cada uma das sequências pode servir muito bem para um caso específico e muito mal para outro. O que faz com que este algoritmo não tenha uma complexidade geral, e sim uma complexidade para cada sequência de **gaps**.

:::

???


Implementação
---------

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
:::
???

Hashi
---------

Para criar um parágrafo, basta escrever um texto contínuo, sem pular linhas.

Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md ;` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

;bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
