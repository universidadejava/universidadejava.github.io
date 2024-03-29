---
layout: article
title: "while"
categories: java
author: sakurai
date: 2020-05-09 00:06:00
tags: [java, loop, while, repetição]
published: true
excerpt: A estrutura while executa um bloco de instruções enquanto uma determinada condição for verdadeira.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Estruturas de repetição

As estruturas de repetição permitem especificar um bloco de instruções que será executado tantas vezes, quantas especificadas pelo desenvolvedor.

## while

A estrutura while executa um bloco de instruções enquanto uma determinada condição for **verdadeira** (true).

{% highlight java %}
while(condição)  {
		< instruções >
}
{% endhighlight %}

Exemplo:

{% highlight java %}
/**
 * Exemplo de estrutura de repetição while.
 */
public class ExemploWhile {
  public static void main(String[] args) {
    int i = 0;

    while(i < 10) {
      System.out.println(++i);
    }
  }
}
{% endhighlight %}

Neste caso, serão impressos os valores de **1 a 10**, e depois quando a variável i possuir o valor **11** a condição do **while** será **falso** (false) e sua estrutura não é mais executada.

{% highlight java %}
C:\>javac ExemploWhile.java
C:\>java ExemploWhile
1
2
3
4
5
6
7
8
9
10
{% endhighlight %}

No vídeo a seguir mostramos passo a passo como você pode usar a estrutura de repetição **while**:

<iframe width="420" height="315" src="https://www.youtube.com/embed/gouKsYVnUTI" frameborder="0" allowfullscreen></iframe>