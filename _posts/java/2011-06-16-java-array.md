---
layout: article
title: "Java - Arrays"
categories: java
author: sakurai
date: 2011-06-16 22:00:00
tags: [java, array, vetor]
published: true
excerpt: Entenda como declarar, incializar e acessar os elementos de um vetor no Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

Segundo a definição mais clássica da informática, um vetor é uma estrutura de dados homogenia, ou seja, todos os elementos de um vetor são do mesmo tipo.

A estrutura básica de um vetor é representada por seu nome e um índice, que deve ser utilizado toda a vez que se deseja acessar um determinado elemento dentro de sua estrutura. É importante ressaltar que todo o vetor possui um tamanho fixo, ou seja, não é possível redimensionar um vetor ou adicinar a ele mais elementos do que este pode suportar. Em Java a posição inicial do vetor é definida pelo valor zero.

### Declaração do vetor

Para se declarar um vetor, devemos informar ao menos seu nome e o tipo de dado que este irá armazenar. Em Java, este tipo de dado pode ser representado tanto por um tipo primitivo como por uma classe qualquer, lembrando que as regras de herança também são válidas para vetores.

Exemplo:

{% highlight java %}
int[] vetorDeInteiros;
float[] vetorDeFloat;
String[] vetorDeString;
long[] vetorDeLong;
{% endhighlight %}

É importante ressaltar que um vetor em Java torna-se um objeto em memória, mesmo que ele seja um vetor de tipos primitivos.

### Inicialização de dados do vetor

Uma vez que um vetor torna-se um objeto em memória, sua inicialização é muito semelhante à de um objeto normal. Uma vez que um vetor é uma estrutura de tamanho fixo, esta informação é necessária a sua inicialização.

{% highlight java %}
int[] vetorDeInteiros = new int[4];
float[] vetorDeFloat = new float[5];
String[] vetorDeString = new String[6];
{% endhighlight %}

Assim como uma variável comum, também é possível inicializar um vetor que já foi declarado anteriormente, conforme exemplo a seguir:

{% gist 484fdcdc3fe2ae8a525d ExemplosVetores.java %}

Assim como um objeto, um vetor deve ser inicializado antes de ser utilizado. Uma chamada a um índice de um vetor não inicializado gera uma exceção.

Existe outra forma de se inicializar vetores já com valores em cada uma de suas posições, para isto basta utilizar chaves da seguinte maneira:

{% gist 484fdcdc3fe2ae8a525d ExemplosVetores.java %}

Note que esta forma de inicialização é bastante prática, porêm não deixa clara a quantidade de elementos que há no vetor obrigando o assim que você conte a quantidade de elementos para saber o tamanho do vetor.

### Acesso aos elementos do vetor

Consideremos o código do exemplo anterior, a representação do seu vetor se daria da seguinte forma:

**índice** | 0 | 1 | 2 | 3 | 4
**vetor** | 2 | 5 | 4 | 8 | 5

Logo, para acessar os valores de cada uma das posições deste vetor você deve utilizar o seu índice correspondente dentro de colchetes, assim como segue:

{% gist 484fdcdc3fe2ae8a525d ExemplosVetores.java %}

Com isso teríamos a seguinte saída em tela:

{% highlight java %}
C:\>javac ExemplosVetores.java
C:\>java ExemplosVetores
Elemento do indice 2 = 4
Elemento do indice 4 = 5
{% endhighlight %}

Lembre-se que o primeiro índice do vetor é sempre zero, logo, seu último elemento é sempre igual ao tamanho do vetor menos um.
