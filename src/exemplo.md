Algoritmo de Knapsack
======

Imagine o seguinte contexto:

Um minerador, depois de muitos anos trabalhando em um centro de garimpo, conseguiu encontrar inúmeras pedras preciosas e metais raros. Todavia, ele possui apenas uma pequena mochila que comporta apenas uma certa quantidade de peso sem arrebentar.

Por termos de simplicidade, ele não possui autorização para quebrar as jóias em outras menores para realizar novas combinações, permitindo-o acumular apenas as jóias brutas, assim como as encontrara.

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

??? Checkpoint
Suponha que o mineiro, com uma mochila que comporta até 20kg, encontre três itens com as seguintes propriedades:
* Item 1: Peso = 14kg; Valor = 10;
* Item 2: Peso = 7kg;  Valor = 4;
* Item 3: Peso = 8kg;  Valor = 7;

Qual seria o **subconjunto ótimo** destes itens, isto é, aquele que que juntos na mochila, retornem o maior valor possível?

**DICA**: Nem sempre os itens de maiores valores pertencerão ao subconjunto ótimo...

::: Gabarito
![](tree.jpg)

O Item 1, apesar de possuir o maior valor bruto (10), não se encontra dentro do subconjunto ótimo, curioso não?
:::
???


Como pode-se averiguar, as soluções em vermelho não se configuram como possíveis seja por ultrapassarem o peso máximo da mochila, ou por não incluir nenhum item dentro desta.

Entretanto, a solução ótima (aquela cujo valor agregado é maximo), não possui o item que tem o valor unitário máximo... 

Isso advém do fato de se observar que, apesar do *Item 1* ter um valor alto, seu peso é grande demais para conseguir comportar este item com qualquer um dos outros dois possíveis. Dessa forma, uma combinação de *Item 2* e *Item 3*, retorna um valor agregado maior, do que o *Item 1* sozinho. Esse ponto mostra que por vezes, nem sempre escolher o item de maior valor primeiro, pode ser certeza de se escolher uma solução ótima...


??? Checkpoint
Qual seriam as formas de se obter o valor máximo (conjunto ótimo) de itens na condição acima?

::: Gabarito
O valor máximo que pode ser obtido de n itens é o máximo dos dois valores a seguir:

* Valor máximo obtido por n-1 itens e peso `md P` da mochila (excluindo n-ésimo item).

* Valor do n-ésimo item mais o valor máximo obtido por n-1 itens e `md P` da mochila menos o peso do n-ésimo item (incluindo o n-ésimo item).
:::
???

Seguindo os dois passos anteriores, podemos pensar em uma solução recursiva para esse problema. Inicialmente, é necessário descartar-se os casos de exceção: 
* Quando o peso da mochila é zero;
* Quando a quantidade de itens é zero;
* Quando o peso de um dado item é maior que o peso total da mochila;

Dessa forma, iremos aplicar uma abordagem recursiva que se inicia do enésimo item, e procura analisar os n-1 itens seguintes:

!!! Aviso
Para o escopo de problemas que pretendemos resolver, considera-se a entrada apenas de valores inteiros, seja para pesos ou valores.
!!!

``` c
int mochila(int peso_max, int pesos[], int valores[], int n) {
    if (n == 0 || peso_max == 0){
        return 0;
    }

    if (pesos[n - 1] > peso_max){
        return mochila(peso_max, pesos, valores, n - 1);
    }
}

```
Em seguida, deve-se analisar o máximo entre os dois casos afirmados no último checkpoint:
* Valor máximo obtido por n-1 itens e peso P da mochila (excluindo n-ésimo item).

* Valor do n-ésimo item mais o valor máximo obtido por n-1 itens e P da mochila menos o peso do n-ésimo item (incluindo o n-ésimo item).

Cria-se uma função auxiliar para o cálculo do máximo dentre dois inteiros:
``` c
int max(int a, int b){
    if (a>b){
        return a;
    }
    return b;
}
``` 

??? Checkpoint
Traduza para código, em termos das entradas dispostas, os dois casos acima de valores possíveis.

::: Gabarito
``` c
    caso1 = mochila(peso_max, pesos, valores, n - 1);

    caso2 = valores[n - 1] + mochila(peso_max - pesos[n - 1], pesos,  valores, n - 1);
```
:::
???
Juntando tudo isso, chegamos a definição do código abaixo, que se mostra efetivo para solucionar o problema requerido.

``` c
int mochila(int peso_max, int pesos[], int valores[], int n) {
    if (n == 0 || peso_max == 0){
        return 0;
    }

    if (pesos[n - 1] > peso_max){
        return mochila(peso_max, pesos, valores, n - 1);
    }

    caso1 = mochila(peso_max, pesos, valores, n - 1);

    caso2 = valores[n - 1] + mochila(peso_max - pesos[n - 1], pesos, 
    valores, n - 1);

    return max(caso1, caso2);
}

```

Feito! O algoritmo acima é conhecido por "*Algoritmo Guloso*" ou "*Força-Bruta*", pois explora todas as combinações de n itens, gerando assim, o total de $2^n$ possibilidades.

Isso ocorre, por conta da função computar os mesmos subcasos a cada recursão, deixando assim a complexidade temporal da solução da forma exponencial, visto que para cada item, existe um subcaso em que o item está e não está:
$$O(2^n)$$

Como pode-se ver, apesar de uma solução possível, parece exaustiva demais para solucionar o problema. Será que existe maneiras mais adequadas de solucionar o mesmo problema?

Um tradeoff com memória
---------
Será possível uma maneira de se "consertar" sucessivas recursões de valores? Isto é, criar uma espécie de memória, para que o algoritmo não precise calcular o peso e valor de um item n vezes?

Para isso, podemos abrir utilizar um pouco mais da memória disponível, onde podemos "guaradar" dados já calculados, e assim economizar caso esse mesmo valor seja sugerido ao cálculo durante a recursão novamente.

??? Checkpoint
Qual seria a estrutura mais adequada para guardamos valores já calculados?

::: Gabarito
Uma matriz de inteiros do tipo 
``` c
int **matriz;
``` 

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
