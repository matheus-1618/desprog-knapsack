Algoritmo de Knapsack
======

Imagine o seguinte contexto:

![](init_problem_img.png)

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

Suponha que temos uma joia Xn, que podemos ou não colocar em nossa mochila de capacidade $P$. 
Temos assim dois possíveis cenários para obter a **configuração ótima** da mochila: 

1. A joia Xn pertence a solução de **configuração ótima** da nossa mochila e devemos coloca-lo.


2. O joia Xn não pertence a **configuração ótima** da mochila e , portanto ,  não devemos inseri-lo.


??? Checkpoint

Qual condição deve ser necessáriamente satisfeita para que a joia esteja dentro da mochila?

**DICA**: Pense no conceito de capacidade.

::: Gabarito

Para a joia está na mochila é necessário que seu peso seja inferior a capacidade da mochila. Ou seja, a joia $i$ pode está na mochila se $peso(i) < P$ , onde P é a capacidade dessa mochila.
:::

???

Essas duas possibilidades devem ser analisadas para cada um dos itens, com intuito de se obter o **subconjunto ótimo** da solução. 

Falamos bastante sobre "Configuração ótima" , "Subconjunto ótimo" , "Solução ótima" , mas o que exatamente seria isso? 

O **subconjunto ótimo** nada mais é que o conjunto de jóias, que juntos tem o maior valor agregado e que a soma de seus pesoas não ultrapassa a determinação de peso da mochila!

Ata! Beleza , beleza , dicionário atualizado ! Vamos agora tentar por meio de um exercício desenvolver a configuração ótima da mochila do mineiro na seguinte situação que se segue.

Suponha que o mineiro, com uma **mochila que comporta até 20kg**, encontre três itens com as seguintes propriedades:

<img src="questao1_guloso.png" alt="questao1" width="450" style="display: block; margin: 0 auto"/>
&nbsp;


??? Exercício

Vamos começar bem do começo ... 

Olhando para a tabela , liste todos os subconjuntos de joias você consegue formar, **sem se importar se o subconjunto ultrapassa ou não a capacidade da mochila**.

**DICA**: Pegue papel e caneta e tente listar todas as possiveis configurações da mochila.

::: Gabarito

* Subconjunto vazio : [ ]

* Subconjuntos de 1 joia apenas : [1] , [2] , [3]

* Subconjuntos de 2 joias : [1 , 2] , [1 , 3] , [2 , 3]

* Subconjunto de 3 joias : [1 , 2 , 3]

:::
???

??? Checkpoint 

Qual elemento parece ter sido adicionado ao conjunto [1] para obtermos o subconjunto [1,2] ? 

Busque asssociar sua resposta com os cenários possíveis de ocorrer quando temos um joia Xn para ser colocado na mochila.


::: Gabarito


Tendo a joia [1] na mochila, foi adicionada a joia [2] para formar o subconjunto [1,2] de joias  na mochila.

Podemos visualizar isso por meio de um diagrama árvore como o mostrado abaixo.


![](inicia_arvore.png)

Analisando qual o peso da mochila nas duas situações percebemos que o subconjunto [1,2] não é viável quando consideramos a capacidade da mochila de 20kg.

:::
???

A ideia de construir uma árvore  (abordada no checkpoint anterior) para a representação dos subconjuntos possíveis de joias na mochila parece ser uma ideia bem interessante e visual para entendermos o problema.

A árvore a seguir busca formar todos os subconjuntos possíveis de joias dentro da mochila utilizando a ideia citada no checkpoint, mas ela esta incompleta ...

<img src="arvore_incompleta.png" alt="arvore incompleta" width="600" style="display: block; margin: 0 auto"/>
&nbsp;

??? Checkpoint

Tente completar a árvore, adicionando as condições em que joia [3] estaria dentro ou fora da mochila, seguindo o mesmo raciocínio do inicio da árvore.

**DICA** : No ultimo andar de construção dessa árvore vocẽ deve obter todos os subconjuntos possíveis de configuração da mochila. 

::: Gabarito
![](tree.jpg)


Observe que as configurações pintadas de vermelho não se configuram como soluções possíveis quando consideramos a capacidade da mochila , ou por ultrapassar essa capacidade ou por não incluir nenhum item dentro desta.

A configuração pintada de verde parece apresentar o maior valor agregado respeitando a capacidade máxima da mochila.

O Item 1, apesar de possuir o maior valor bruto (10), não se encontra dentro do subconjunto ótimo, curioso não?
:::
???

Como é possível observar , a solução ótima encontrada (aquela cujo valor agregado é maximo), não possui o item que tem o valor unitário máximo... 

Isso advém do fato de se observar que, apesar do *Item 1* ter um valor alto, seu peso é grande demais para conseguir comportar este item com qualquer um dos outros dois possíveis. Dessa forma, uma combinação de *Item 2* e *Item 3*, retorna um valor agregado maior, do que o *Item 1* sozinho. 

Esse ponto mostra que por vezes, nem sempre escolher o item de maior valor primeiro, pode ser certeza de se escolher uma solução ótima...


Nesse ponto você deve ser capaz de entender qual a ideia por traz do problema da mochila e uma ideia inicial de como é possível resolve-lo formando o melhor subconjunto de itens possível.
Vamos agora tentar transformar os conceitos aprendidos em uma implementação recursiva.

??? Checkpoint

Lembra-se das possíveis situações que temos ao possuirmos uma joia $Xn$ e uma mochila de capacidade $P$?
Essa joia pode ou não está na solução ótima da mochila.

Vamos tentar transforma essa situação em algo um pouco mais semelhante a uma passo recursivo. 

Para isso considere uma função $F(x,C)$ que representa a soma de valores das joias da configuração ótima quando possuimos **x jóias a serem adicionadas e uma mochila de capacidade c**.


**DICA**: Lembre-se que cada joia $n$ que pode ser adicionada na mochila possui um valor associado $V(n)$ e um peso associado $P(n)$.
          
          - Se eu adiciono um item na mochila , eu acrescento seu valor a soma de valores, e diminuo a capacidade C da mochila.
          
          - Se eu não adiciono um item x , eu ainda possui x-1 itens para adicionar a uma mochila de capacidade C.

::: Gabarito

![](ideacaoMatematica.png)

1. A joia $X_n$ está na solução ótima :  Valor(n) + $F(n-1 , P - P(n))$ , para isso o item deve possuir $Peso(n) < Capacidade \ mochila$

2. A joia $X_n$ não está na solução ótima :  $F(n-1 , P)$

:::

???

Seguindo os passos anteriores, podemos pensar em uma solução recursiva para esse problema. Inicialmente, é necessário descartar-se os casos de exceção: 
* Quando o peso da mochila é zero;
* Quando a quantidade de itens é zero;
* Quando o peso de um dado item é maior que o peso total da mochila;

Dessa forma, iremos aplicar uma abordagem recursiva que se inicia do enésimo item, e procura analisar os n-1 itens seguintes:

!!! Aviso
Para o escopo de problemas que pretendemos resolver, considera-se a entrada apenas de valores inteiros, seja para pesos ou valores.
!!!

``` c
int mochila(int peso_max, int pesos[], int valores[], int n) {
    if (n == 0 || peso_max == 0){        // Nenhum item a ser adicionado ou Mochila de capacidade zero
        return 0;
    }

    if (pesos[n - 1] > peso_max){       // Peso do item maior que a capacidade da mochila 
        return mochila(peso_max, pesos, valores, n - 1);
    }
}

```

Vamos agora abordar o caso que um item **cabe na mochila** , ou seja , tem seu $peso < capacidade$ devemos analisar se devemos ou não adiciona-lo na mochila. Será que devemos adiciona-lo na mochila?

Para respondermos essa pergunta temos que questionar : o item $i$ está  na configuração ótima da mochila ? 

Tomamos essa decisão analisando a configuração ótima da mochila nas duas situações :

1 - O item **está na mochila**.

2 - O item **não está na mochila**.

A situação em que a configuração resulta na **maior soma de valores** dentro da mochila será escolhida.


Como estamos pensando na **maior soma entre duas configurações da mochila**, faz sentido criar uma função auxiliar para o cálculo do máximo dentre dois valores inteiros:

``` c
int max(int a, int b){
    if (a>b){
        return a;
    }
    return b;
}
``` 

??? Checkpoint

Em um **checkpoint anterior** ja abordamos a situação em que uma joia $X_n$ pertence a configuração ótima da mochila e a situação em que ela não pertence. 

Pense de que forma recursiva podemos chamar a função **mochila( ... )** no código, de forma a abordar a situação em que o item $X_n$ está na configuração ótima da mochila (2) e o caso em que ele não está (1).

::: Gabarito
```c 
    int caso1 = mochila(peso_max, pesos, valores, n - 1);

    int caso2 = valores[n - 1] + 
                mochila(peso_max - pesos[n - 1], pesos,  valores, n - 1);
```
:::
???

!!!Importante
Toda vez que tomamos a decisão de adicionar uma joia $X_n$ passamos a ter que observar qual seria a solução ótima de um mochila com capacidade $P - P(n)$ com apenas $n-1$ objetos para inserir ! 

**É como se estivessemos analisando uma nova mochila**!
!!!

Juntando tudo isso, chegamos a definição do código abaixo, que se mostra efetivo para solucionar o problema requerido.

``` c
int mochila(int peso_max, int pesos[], int valores[], int n) {
    if (n == 0 || peso_max == 0){
        return 0;
    }

    if (pesos[n - 1] > peso_max){
        return mochila(peso_max, pesos, valores, n - 1);
    }

    int caso1 = mochila(peso_max, pesos, valores, n - 1);

    int caso2 = valores[n - 1] + mochila(peso_max - pesos[n - 1], pesos, 
    valores, n - 1);

    return max(caso1, caso2);
}

```

<img src="levaOuro.png" alt="mineiro feliz" width="200" style="display: block; margin: 0 auto"/>
&nbsp;

Feito! Construímos um **algoritmo recursivo** que resolve o problema da mochila explorando todas as combinações/subconjunto de n itens a serem analisados.

??? Checkpoint
Qual é a complexidade temporal do **algoritmo recursivo** construido?

::: Gabarito
$$O(2^n)$$
Isso ocorre, por conta da função computar os mesmos subcasos a cada recursão, deixando assim a complexidade do tipo exponencial, visto que para cada item, existe um subcaso em que o item se engloba e não se engloba, gerando assim, o total de $2^n$ possibilidades.

<img src="chateado.png" alt="mineiro feliz" width="150" style="display: block; margin: 0 auto"/>
&nbsp;


:::
???

Como pode-se ver pela solução do checkpoint anterior, apesar de uma solução possível, parece exaustiva demais para solucionar o problema. Será que existe maneiras mais adequadas de solucionar o mesmo problema?

Utilizando Programação dinâmica
---------

Acabamos de desenvolver um Algoritmo recursivo que soluciona o nosso problema da mochila, mas como foi possível observar pela complexidade desse Algoritmo ele não se apresenta algo viável quando estamos falando de uma entrada com muitos itens (n grande). 

Dito isso, vamos começar a pensar em outra abordagem utilizando a programação conhecida como **dinâmica**.  

A programação dinâmica é um método de desenvolvimento que busca encontrar a solução de vários subproblemas para, daí então, encontrar a solução do problema geral, em uma abordagem chamada **bottom-top** (ao contrário do que se ocorre com a proposta recursiva visto anteriormente).  A principal característica distintiva entre esse método e o Algoritmo recursivo é a utilização de uma tabela que armazena as soluções das várias subinstâncias do problema. Por conta disso, esse estratégia pode ser vagamente definida como **"recursão com apoio de uma tabela"**.

!!! Aviso

Os conceitos necessários para decisão se um item $X_n$ está ou não na configuração ótima da mochila foram desenvolvidos na implementação do Algoritmo recursivo.

Esses princípios serão utilizados também para o desenvolvimento dessa abordagem então não prossiga se não tiver entendido bem o handout até aqui!

!!!

Vamos começar a implementar o código que resolve nossa questão de forma dinâmica. Como dito anteriormente a principal diferença entre a abordagem recursiva e esssa é a utilização de um tabela para armazenar as soluções das subinstancias do problema.

Vamos iniciar a implementação criando uma a função com a definição dessa tabela:

``` c

int knapSack(int W, int wt[], int val[], int n)
{
    int F[n + 1][W + 1];
    
    ...

}
```

Considere que a `md função knapSack` recebe os seguintes argumentos:

* $\textbf{W}$  : Inteiro que representa o ṕeso da mochila.

* $\textbf{wt[]}$  : Vetor de inteiros que representa os pesos dos possíveis objetos a serem colocados na mochila. 

* $\textbf{val[]}$  : Vetor de inteiros que representa os valores dos objetos que podem ser colocados na mochila.

* $\textbf{n}$   : Inteiro que representa o número de objetos.

Na função definida acima vemos que $F(x,c)$ definida anteriormente está representada em forma de matriz. Essa matriz pode ser interpretada como uma **tabela que será preenchida** para a determinação do valor na solução ótima da mochila.

!!! Lembrete
$F(x,c)$ representa a soma de valores das joias da configuração ótima quando possuimos x jóias a serem adicionadas e uma mochila de capacidade c.
!!!

![](table1.png)

!!! Aviso

* Cada coluna da tabela  mostrada representa a condição em que temos **n objetos (n1,n2,n3...) a serem adicionados em um mochila de capacidade específica p**. 

* Cada linha esta representando a situação em que temos apenas **um objeto específico que pode ou não ser adicionado a mochilas de diversas capacidades p (p1,p2,p3 .. pn)**.

!!!

Voltemos novamente ao problema do mireiro e suas joias, imagine a situação em que ele possui os seguintes objetos com seus respectivos pesos e valores:

<img src="prog_dinamica.png" alt="prog dinamica" width="450" style="display: block; margin: 0 auto"/>
&nbsp;


??? Checkpoint

Utilizando os dados do problema apresentados , qual a dimensão na matriz $F[n + 1][W + 1]$ nesse caso ?

**DICA** : Pense na definição dessa matriz.

::: Gabarito
A matriz deve possuir **4 linhas** (0 a 3 objetos) e **7 colunas** (pesos de 0 a capacidade da mochila)

![](tabela2.png)
:::

???

??? Checkpoint

O que a primeira linha e a primeira coluna da tabela $F[n+1][W+1]$ representa?

::: Gabarito

1 . A primeira linha da matriz representa a situação em que temos uma mochila de capacidade P, mas nenhum objeto para adicionar ($F(0,P)$). Nesse caso, o valor adicionado a mochila é zero!

2 . A primeira coluna da matriz representa a situação em que temos n objetos que podem ser adicionados em uma mochila de capacidade $P = 0$  ($F(n,0)$) . Nesse caso também não consigo adicionar nenhum objeto. 

:::

???

??? Exercício

Sabendo que a primeira linha e coluna da matriz precisa ser zerada, de que forma podemos representar isso em código na `md função knapSack` ?

**DICA** : Construa uma estrtutura que "varre" a matriz passando por todos os seus elementos.

::: Gabarito

``` c

int knapSack(int W, int wt[], int val[], int n)
{
    int F[n + 1][W + 1];
    
    for (int i = 0; i <= n; i++)   // Varre objetos (linhas)
    {
        for (int w = 0; w <= W; w++)   // Varre capacidades (colunas)
        {
            
            if (i == 0 || w == 0){
                F[i][w] = 0; 
            }

            ...

        }
    }

}
```

:::

???

Legal, preenchemos a primeira linha e coluna! Após a implementação acima sua tabela no exemplo prático do mineiro estaria parecida com a mostrada abaixo.

![](table3.png)


??? Checkpoint

Tente explica o que cada elemento $(i,j)$ da tabela acima representa, utilizando o exemplo do mineiro como base.

::: Gabarito

**Elemento (1,1)** : Mochila de capacidade 1 com o objeto 1 , de peso = 4 e valor = 8, para adicionar.

**Elemento (1,2)** : Mochila de capacidade 2 com o objeto 1 , de peso = 4 e valor = 8, para adicionar.

**Elemento (1,3)** : Mochila de capacidade 3 com o objeto 1 , de peso = 4 e valor = 8, para adicionar.

...

**Elemento (3,7)** : Mochila de capacidade 7 com o objeto 3 , de peso = 2 e valor = 4, para adicionar.

:::

???

Bem, preeencher a primeira linha e coluna da tabela não pareceu muito desafiador. Vamos tentar agora preencher o seu interior.Para preeencher os demais elementos da matriz vamos ter que aplicar o que vimos anteriormente com a função da solução ótima $F(i,p)$.

$$F(i,p) = 
\begin{cases}
Max( F(\ i-1 \ , \ P - P(i) \ ) + V(i) , F(\ i-1 \ , \ P \ ) ) \ \text{, se P(i) <= P}\\
F(\ i-1 \ ,\ P \ ) \ \text{, se P(i) > P}\
\end{cases}
$$

??? Checkpoint

Indique como ficaria a expressão acima para o elemento $(1,1)$ da tabela, ou seja, caso em que temos o objeto 1 e a mochila de capacidade 1. 

Indique também qual das condições determinará o valor do elemento $(1,1)$ da tabela.

**DICA** : Que nesse ponto estamos considerando que a capacidade da mochila é 1! Não 7 , que é a capacidade máxima da mochila.

::: Gabarito

$$F(1,1) = 
\begin{cases}
Max( F(\ 1-1 \ , \ 1 - P(1) \ ) + V(1) , F(\ 1-1 \ , \ 1 \ ) ) \ \text{, se P(i) <= 1}\\
F(\ 1-1 \ ,\ 1\ ) \ \text{, se P(1) > 1}\
\end{cases}
$$ 

Como 4 é maior que 1 , **o objeto 1 não cabe na mochila de capacidade 1**. Assim, o valor $F(1,1)$ fica definido como:

$$F(1,1) = F(0,1) = 0 $$

:::

???


??? Exercício

Usando o raciocínio do checkpoint anterior, como ficaria a tabela inteiramente preenchida?

::: Gabarito

:table

:::

??? 

Após esse checkpoint você deve ter conseguido chegar no subconjunto de que objetos devem ser adicionado na mochila do mineiro para obtermos o maior valor que a "mochila" consegue guardar , em seu caso ótimo.

Por fim, falta finalizarmos o código da `md função knapSack` adicionando a implementação que permite o preenchimento dos demais elementos da matriz.

??? Exercício

Implemente o preenchimento dos demais elementos da matriz no código e faça a função retornar o maior valor guardado na mochila em seu caso ótimo.

::: Gabarito

``` c

int knapSack(int W, int wt[], int val[], int n)
{
    int i, w;
    int F[n + 1][W + 1];
 
    for (i = 0; i <= n; i++)
    {
        for (w = 0; w <= W; w++)
        {
            if (i == 0 || w == 0)
                F[i][w] = 0;
            else if (wt[i - 1] <= w)    // Objeto cabe na mochila
                F[i][w] = max(val[i - 1]
                          + F[i - 1][w - wt[i - 1]],
                          F[i - 1][w]);
            else
                F[i][w] = F[i - 1][w]; // Objeto não cabe na mochila
        }
    }
 
    return F[n][W];
}
```

:::

???

Pronto ! Agora temos uma implementação da solução do problema da mochila por meio de programação dinâmica !

??? Checkpoint
Qual é a complexidade temporal do **algoritmo de programação dinâmica** construído?

::: Gabarito
$$O(n*W)$$

Onde 'n' é o número de elementos que podem ser adicionados na mochila e 'W' é a capacidade da mochila.
:::
???

Estamos quase no fim ...  

No Algoritmo que implementamos precimos de espaço na memória para armazenar uma matriz [N° elementos - 1][Capacidade -1]. 

Será que não há um jeito melhor de guardarmos as configurações ótima da mochila ?

Melhorando um pouco mais..
---------

Vamos voltar um pouco e analisar melhor uma das tabelas que montamos anteriormente:

![](table/table8.png)

Quando olhamos linha a linha dessa tabela observamos que alguns elementos da matriz parecem ociosos ou desnecessários para chegarmos a conclusão final. 

Por exemplo, a linha 0 e coluna 0 , ocupam espaço na memória simplesmente para tratar os casos em que a mochila possui capacidade nula ou nenhum item para inserir. 

Outro exemplo é o elemento (1,2) na matriz , nesse caso estarimos obtendo a situação ótima quando tempo o elemento 1 , de peso 4kg , e a mochila de capacidade 2kg. O elemento (1,2) não cabe nessa configuração da mochila mas ainda sim estamos alocando espaço na memória para indicar isso na matriz. **Será que isso é mesmo necessário?**

Uma alternativa viável, pode ser a utilização de um vetor ao invés de uma matriz inteira.


??? Checkpoint
Qual dimensão reduziríamos da matriz? 

A de pesos possíveis (0 até W) ou de elementos (0 até n-1)?

::: Gabarito
A de elementos.
É possível retirar esse dimensão da estrutura de armazenamento, visto que o único valor que representa uma restrição é o **peso** do conjunto de itens analisado.

Assim pode-se analisar para cada combinação de itens que gerem dado peso, qual é o subconjunto ótimo (aquele que retorna maior valor), e armazenar somente se o atual for maior que aquele que já está na posição.
:::
???

Dessa maneira, simplifica-se e muito a questão de armazenamento dos valores. Diminuindo a complexidade de espaço antes de $O(n*W)$ para $O(n)$.

Mas como, efetivamente, essa idéia pode ser implementada na prática?

Vamos pensar de maneira mais simples...

??? Checkpoint
Como devemos percorrer esse vetor, com objetivo de **sempre** armazenar os valores que retornem subconjunto ótimo?

**DICA**: Pense na restrição das combinações de peso.

::: Gabarito
Idealmente da direita pra esquerda. Pois ao analisar o dado peso (entre 0 e Peso da Mochila), é possível analisar se já possuímos a solução ótima, ou se existe algum outro conjunto dentro da restrição de peso que ainda tenha valor maior, iniciando da situação que a mochila está vazia (Peso Máximo) até quando ela chega em sua capacidade máxima (0 ou algum valor menor que Peso Máximo).
:::
???

Assim, para cada posição a ser iterada, devemos comparar a o valor alocado na posição atual com uma situação do elemento analisado, gerando assim, duas possibilidades de escolha:

* O valor atual guardado na posição do vetor;
* O valor dado pelo do peso analisado menos o peso do elemento i somado pelo o valor agregado do elemento i.

A segunda alternativa, corresponde a simplesmente adicionar um item na mochila, visto que o peso resultante será o peso restante na mochila, após alocarmos o elemento i. Muito parecido com o que fazemos anteriormente, não? 

!!!Lembre-se
Toda vez que tomamos a decisão de adicionar um elemento $X_n$ passamos a ter que observar qual seria a solução ótima de um mochila com capacidade $P - P(n)$ com apenas $n-1$ objetos para inserir ! 

**É como se estivessemos analisando uma nova mochila**!
!!!

Ou seja, estamos sendo capazes de manter a mesma lógica de análise de subcasos, mas usando muito menos espaço para alocar os valores.


![](otimizando_memoria.png)


??? Checkpoint
Em qual posição do vetor fica alocada o subconjunto ótimo?

::: Gabarito
Na última, correspondente ao peso máximo da mochila, visto que ao iterarmos da direita pra esquerda, analisando item a item, tende-se a alocar os valores em ordem crescente, devido a substituição decorrente da combinação entre os possíveis subcasos, conforme as premisas nos tópicos acima. Interessante não?
:::
???


Para ver como ocorre na prática essa alocação, suponha o exemplo abaixo, composto por uma mochila com capacidade de **4kg**, e três itens:
* Item 1: Valor = 60, peso = 1kg
* Item 2: Valor = 100, peso = 2kg
* Item 3: Valor = 120, peso = 3kg

Para preencher o vetor de valores, veja a animação abaixo, onde segue-se a premissa de comparar o valor da posição atual com o valor dado pelo do peso analisado menos o peso do elemento i somado pelo o valor agregado do elemento i.

:optimiza


??? Checkpoint
**Desafio**

Implemente a ideia acima em código.

**DICA**: Parta do código de Programação Dinâmica na sessão anterior.

::: Gabarito
```c
int knapSack(int W, int wt[], int val[], int n)
{
    //Inicializando o vetor a ser utilizado
    //Coloque-se zero em todos por fins de simplicidade
    int vetor[W + 1];
    for (int j = 0; j < W + 1; j++){
        vetor[j] = 0;
    }
 
    for (int i = 1; i < n + 1; i++) {
        for (int w = W; w >= 0; w--) {
            //Caso peso analisado seja menor ou igual ao vetor de pesos da mochila
            if (wt[i - 1] <= w)
                // Valor atual guardado no vetor
                int caso1 = vetor[w];
                //Valor retornado pelo peso atual menos o 
                //peso do elemento iterado, mais o valor deste.
                int caso2 = vetor[w - wt[i - 1]] + val[i - 1];
                vetor[w] = max(caso1, caso2);
        }
    }
    return vetor[W]; 
}
```
:::
???

...


Vamos ver se você entendeu ...
---------

??? Exercício - Algoritmo Recursivo

Considere a seguinte configuração de itens que podem ser colocados em uma mochila de capacidade **2Kg**.

|  Itens | Valor | Peso (Kg) |
|----------|----------|----------|
| $1$        | 10       |   1   
| $2$        | 7         | 2   
| $3$        | 2         |  1  

Desenhe a arvore de cada subcaso possível (item adicionado ou não), ilustrando o subcaso ótimo.

::: Gabarito
![](arvore.png)
::: 

???

??? Exercício - Programação Dinâmica

Considere a seguinte configuração de itens que podem ser colocados em uma mochila de capacidade **2Kg**.

|  Itens | Valor | Peso (Kg) |
|----------|----------|----------|
| $1$        | 10       |   1   
| $2$        | 7         | 2   
| $3$        | 2         |  1   
  

Desenhe a tabela (N° itens x Capacidade) dos subconjuntos ótimos de configuração da mochila. 

::: Gabarito
![](questao_tabela_revisa.png)
::: 

???


Conteúdo Extra : Melhorando o Algoritmo recursivo
---------
Será possível uma maneira de se "consertar" sucessivas recursões de valores? Isto é, criar uma espécie de memória, para que o algoritmo evite  calcular repetidamente o peso e valor de um item n vezes?

Para isso, pode-se  utilizar um pouco mais da memória disponível, onde pode-se "guardar" dados já calculados, e assim evitar o cálculo de casos já abordados em outros subcasos.

??? Checkpoint
Qual seria uma possível estrutura para armazenar valores já calculados?

::: Gabarito
Uma matriz de inteiros do tipo 
``` c
int **valores;
``` 

Onde as linhas são indicações do **n** elementos, e as colunas representações dos **P** pesos associados ao subcojunto.
:::
???

Dessa forma, devemos então sempre analisar duas condições a cada item analisado:
* Se o item já foi analisado, retorna-se esse item sem a necessidade de se recalcular esse subconjunto;

* Se o item não foi analisado, analisa-se este dentro das conjunturas dos dois casos abordados na recursão.

??? Checkpoint
Qual seria a condição inicial da matriz de valores para se possibilitar uma diferenciação entre um valor já calculado e outro que não?

::: Gabarito
Inicializar a matriz, com um valor que não seja possível de se obter para a gama de problemas a serem resolvidos.

No contexto do Minerador (o escopo de problemas que pretendemos aqui resolver) não faz sentido estas possuírem pesos ou valores que sejam menores que zero (negativos). Dessa forma, podemos iniciar a matriz com um números negativos, que nos permitam diferenciar entre valores já obtidos e outros não.
:::
???

Dessa forma, uma primeira modificação a se realizar é:
``` c
int mochila(int peso_max, int pesos[], int valores[], int n) {
    int **valores_matriz;
    for (int i = 0; i < n; i++){
        for (int j = 0; j < peso_max + 1; j++){
            valores_matriz[i][j] = -1;
        }
    }
    /* Resto do código */
}
```

Certo, agora temos uma matriz que irá alocar para dados **n** elementos, **P+1** pesos possíveis, que irão nos economizar cálculos de subcasos já analisados para tal combinação.

Agora temos que combinar o espaço possibilitado por essa matriz com a estratégia recursiva.


??? Checkpoint
Crie uma função auxiliar chamada *mochila_recusiva*, que receba os mesmos parâmetros de mochila, além da matriz criada, e que resolva os subcasos ainda não calculados via recursão e retorne os calculados caso os encontre na matriz.

**DICA**: Boa parte do código pode ser aproveitado do algoritmo recursivo já construído, lembre-se sempre de armazenar o valor calculado em seus respectivo local na matriz.

::: Gabarito
``` c
int mochila_recursiva(int peso_max, int pesos[], int valores[],
                     int n, int **matriz_valores) {
    if (n == 0 || peso_max == 0){
        return 0;
    }

    //Valor já foi encontrado na matriz
    if(matriz_valores[n][peso_max] != 1){
        return matriz_valores[n][peso_max];
    }

    if (pesos[n - 1] > peso_max){
        matriz_valores[n][peso_max] = mochila_recursiva(peso_max, pesos, 
                                            valores, n - 1,matriz_valores);
        return matriz_valores[n][peso_max];
    }

    caso1 = mochila_recursiva(peso_max, pesos, valores, n - 1);

    caso2 = valores[n - 1] + mochila_recursiva(peso_max - pesos[n - 1], pesos, 
    valores, n - 1);

    matriz_valores[n][peso_max] = max(caso1, caso2);
    return matriz_valores[n][peso_max];
}
```
:::
???

Assim, basta realizar uma chamada desssa função dentro da função *mochila*:

``` c
int mochila(int peso_max, int pesos[], int valores[], int n) {
    int **valores_matriz;
    for (int i = 0; i < n; i++){
        for (int j = 0; j < peso_max + 1; j++){
            valores_matriz[i][j] = -1;
        }
    }
    return mochila_recursiva(peso_max, pesos, valores, n - 1, valores_matriz);
}
```
Feito! Conseguimos combinar a orquestração recursiva da abordagem top-down para varrer todos os casos possíveis, com a regalia de não termos de ter que recalcular subcasos já obtidos.

??? Checkpoint
Qual a complexidade temporal do código acima?
::: Gabarito
$$O(pesoMax*n)$$
Visto que no pior caso, percorre-se todas as entradas da matriz de linhas **n**, e colunas **pesos**.
???

Dessa maneira, conseguimos uma abordagem um pouco mais eficiente em termos de complexidade temporal.
??? Checkpoint
Compare a complexidade de espaço das duas propostas abordadas.

**DICA**: Em recursões utilizadas para **n** elementos, cerca de **n** espaços de pilha auxiliar são alocados durante a execução do código.
::: Gabarito
| Proposta 1 | Proposta 2 |
|----------|----------|
| $O(n)$        | $O(pesoMax*n)$         |
???

![](end.gif)