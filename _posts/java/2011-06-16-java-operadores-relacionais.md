---
layout: article
title: "Java - Operadores relacionais"
categories: java
author: sakurai
date: 2011-06-16 20:30:00
tags: [java, operadores, relacionais]
published: true
excerpt: Conheça os operadores relacionais do Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

Os resultados dos operadores relacionais são do tipo boolean.

## Operador Maior Que

Símbolo > é chamado de maior que.

{% highlight java %}
<operando1> > <operando2>
{% endhighlight %}

Retorna true se o valor do operando1 for maior que o valor do operando2, caso contrario retorna false.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorMaiorQue.java %}

Neste caso será impresso true, pois o valor da variável a é maior que o valor da variável b.

{% highlight java %}
C:\>javac OperadorMaiorQue.java
C:\>java OperadorMaiorQue
true
{% endhighlight %}

## Operador Maior Que

Símbolo < é chamado de menor que.

{% highlight java %}
<operando1> < <operando2>
{% endhighlight %}

Retorna true se o valor do operando1 for menor que o valor do operando2, caso contrario retorna false.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorMenorQue.java %}

Neste caso será impresso false, pois o valor da variável a não é menor que o valor da variável b.

{% highlight java %}
C:\>javac OperadorMenorQue.java
C:\>java OperadorMenorQue
false
{% endhighlight %}

## Operador Igualdade

Símbolo == é chamado de igualdade.

{% highlight java %}
<operando1> == <operando2>
{% endhighlight %}

Retorna true se o valor do operando1 for igual ao valor do operando2, caso contrario retorna false.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorIgualdade.java %}

Neste caso será impresso true, pois o valor da variável a é igual ao valor da variável b.

{% highlight java %}
C:\>javac OperadorIgualdade.java
C:\>java OperadorIgualdade
true
{% endhighlight %}

## Operador Maior ou Igual Que

Símbolo >= é chamado de maior ou igual que.

{% highlight java %}
<operando1> >= <operando2>
{% endhighlight %}

Retorna true se o valor do operando1 for maior ou igual que o valor do operando2, caso contrario retorna false.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorMaiorIgualQue.java %}

Neste caso será impresso true, pois o valor da variável a é maior que valor da variável b e depois será impresso true novamente, pois o valor da variável a é igual ao valor da variável c.

{% highlight java %}
C:\>javac OperadorMaiorIgualQue.java
C:\>java OperadorMaiorIgualQue
true
true
{% endhighlight %}

## Operador Menor ou Igual Que

Símbolo <= é chamado de menor ou igual que.

{% highlight java %}
<operando1> <= <operando2>
{% endhighlight %}

Retorna true se o valor do operando1 for menor ou igual que o valor do operando2, caso contrario retorna false.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorMenorIgualQue.java %}

Neste caso será impresso true, pois o valor da variável a é menor ou igual ao valor da variável b.

{% highlight java %}
C:\>javac OperadorMenorIgualQue.java
C:\>java OperadorMenorIgualQue
true
{% endhighlight %}

## Operador Diferente

Símbolo != é chamado de diferente.

{% highlight java %}
<operando1> != <operando2>
{% endhighlight %}

Retorna true se o valor do operando1 for diferente do valor do operando2, caso contrario retorna false.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorDiferente.java %}

Neste caso será impresso true, pois o valor da variável a é diferente do valor da variável b e depois será impresso false, pois o valor da variável b é igual ao valor da variável c.

{% highlight java %}
C:\>javac OperadorDiferente.java
C:\>java OperadorDiferente
true
false
{% endhighlight %}
