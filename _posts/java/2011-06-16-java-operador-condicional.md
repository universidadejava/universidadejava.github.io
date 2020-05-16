---
layout: article
title: "Java - Operador Condicional"
categories: java
author: sakurai
date: 2011-06-16 21:34:00
tags: [java, condicional, ternário]
published: true
excerpt: Conheça o operadore condicional do Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## O operador condicional é do tipo ternário, pois envolve três operandos.

Símbolo ? : é utilizado para fazer uma condição if / else de forma simplificada.

{% highlight java %}
<operando1> ? <operando2> : <operando3>
{% endhighlight %}

Se o valor do operando1 for true, então o resultado da condicional é o operando2, se o valor do operando1 for false, então o resultado da condicional é o operando3.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorCondicional.java %}

Neste caso a condição a != b retorna true, então é impresso o valor “diferente”, se o resultado fosse false, então seria impresso o valor “igual”.

{% highlight java %}
C:\>javac OperadorCondicional.java
C:\>java OperadorCondicional
diferente
{% endhighlight %}
