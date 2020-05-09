---
layout: article
title: "Java - Operadores unários"
categories: materiais
author: sakurai
date: 2011-06-15 17:55:00
tags: [java]
published: true
excerpt: Conheça os operadores unários de incremento e decremento do Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

Símbolo ++ é utilizado para incrementar em 1 o valor de uma variável, podendo ser feita das seguintes formas:

{% highlight java %}
++ <variável>
   Primeiro incrementa a variável depois devolve seu valor.

<variável> ++
   Primeiro devolve o valor da variável depois incrementa seu valor.
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorIncremento.java %}

Neste caso a variável a e variável b são inicializadas com 1, mas quando é impresso o valor da variável a imprime 2 enquanto que o valor da variável b imprime 1, e na segunda vez que é impresso ambas variáveis a e b possuem o valor 2.

{% highlight java %}
C:\>javac OperadorIncremento.java
C:\>java OperadorIncremento
2
1
2
2
{% endhighlight %}

Símbolo - - é utilizado para decrementar em 1 o valor de uma variável, podendo ser feita das seguintes formas:

{% highlight java %}
-- <variável>
   Primeiro decrementa o valor da variável, depois devolve seu valor.

<variável> --
   Primeiro devolve o valor da variável, depois ela é decrementada.
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorDecremento.java %}

Neste caso a variável a e variável b são inicializadas com 1, mas quando é impresso o valor da variável a imprime 0 enquanto que o valor da variável b imprime 1, e na segunda vez que é impresso ambas variáveis a e b possuem o valor 0.

{% highlight java %}
C:\>javac OperadorDecremento.java
C:\>java OperadorDecremento
0
1
0
0
{% endhighlight %}
