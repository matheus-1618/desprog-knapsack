Algoritmo de Knapsack
======

Algoritmo de Programação Dinâmica para o Problema da Mochila
---------

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

Mesmo que estas sejam eficazes, será que existe uma solução que **sempre** retorna a solução mais eficaz para este problema?

Isso é o que o o algoritmo de programação dinâmica de **Knapsack** propõe: Trazer a solução mais eficaz e adequada para problemas dessa classe.





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
