Algoritmo de Knapsack
======

Para entendermos melhor a ideia central desse algoritmo, imagine o seguinte contexto:

Um minerador, depois de muitos anos trabalhando em um centro de garimpo, conseguiu encontrar inúmeras pedras preciosas e metais raros. Todavia, ele possui apenas uma pequena mochila que comporta apenas uma certa quantidade de peso sem arrebentar.

??? Checkpoint
Qual seria a forma mais eficaz do mineiro conseguir conseguir levar o máximo valor em jóias, sem arrebentar sua pequena mochila?

::: Gabarito
Algumas soluções imediatas podem ser as seguintes:

* Procurar as peças de maior valor e encher a mochila com as mais valiosas até seu máximo;

* Procurar aquelas que tem menor peso, e pegar a maior quantidade dessas, compensando seu menor valor com maior quantidade;

Todavia, podem existir caminhos melhores...
:::
???

Mesmo que estas sejam eficazes, será que existe uma solução (*ou soluções...*) que **sempre** retorna a solução mais eficaz para este problema?


Isso é o que o o algoritmo de programação dinâmica de **Knapsack** propõe: Trazer a solução mais eficaz e adequada para problemas dessa classe.

Vamos entender alguns subcasos para melhorar a compreensão do problema.

Analisando item a item
---------

Vamos voltar ao caso do mineiro, e tentar imaginar o seguinte contexto: 

A cada jóia dentro da mineradora, divide-se em dois cenários:
* Aquele em que a jóia está na mochila;

* Aquele em que a jóia não está na mochila;

Essas duas possibilidades devem ser analisadas para cada um dos itens, com intuito de se obter o **subconjunto ótimo** da solução, isto é, o determinado conjunto de jóias, que juntos tenham o maior valor agregado e não ultrapassem a determinação de peso da mochila.

Veja abaixo o caso prático, consistente de uma mochila de 20 kg, e três itens (com valores e pesos diferentes) em que se emprega o raciocínio acima para a busca da solução de valor máximo.

![](tree.jpg)

Como pode-se averiguar, as soluções em vermelho não se configuram como possíveis seja por ultrapassarem o peso máximo da mochila, ou por não incluir nenhum item dentro desta.

Entretanto, a solução ótima (aquela cujo valor agregado é maximo), não possui o item que tem o valor unitário máximo (*Item 1*)... 

Isso advém do fato de se observar que, apesar do *Item 1* ter um valor alto, seu peso é grande demais para conseguir comportar este item com qualquer um dos outros dois possíveis. Dessa forma, uma combinação de *Item 2* e *Item 3*, retorna um valor agregado maior, do que o *Item 1* sozinho. Esse ponto mostra que por vezes, nem sempre escolher o item de maior valor primeiro, pode ser certeza de se escolher uma solução ótima...


??? Checkpoint
Qual seriam as formas de se obter o valor máximo (conjunto ótimo) de itens na condição acima?

::: Gabarito
O valor máximo que pode ser obtido de n itens é o máximo dos dois valores a seguir:

* Valor máximo obtido por n-1 itens e peso `md P` da mochila (excluindo n-ésimo item).

* Valor do n-ésimo item mais o valor máximo obtido por n-1 itens e `md P` da mochila menos o peso do n-ésimo item (incluindo o n-ésimo item).
:::
???

Em termos de programação, podemos pensar em uma solução do tipo:

``` c
int knapsack(int peso_max, int pesos[], int valores[], int n) {
    if (n == 0 || W == 0){
        return 0;
    }
    if (pesos[n - 1] > W){
        return knapsack(peso_max, pesos, valores, n - 1);
    }
 
    // Return the maximum of two cases:
    // (1) nth item included
    // (2) not included
    else {
        return max(valores[n - 1]+ knapsack(peso_max - pesos[n - 1],pesos, valores, n - 1),
            knapsack(peso_max, pesos, valores, n - 1));
        }
}

```

??? Checkpoint
Implemente uma solução inicial para que dado `md :`

::: Gabarito

:::
???




Algoritmo de Programação Dinâmica para o Problema da Mochila
---------
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

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

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
