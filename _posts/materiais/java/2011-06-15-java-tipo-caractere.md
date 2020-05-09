---
layout: article
title: "Java - Tipo primitivo de caractere"
categories: materiais
author: sakurai
date: 2011-06-15 15:43:00
tags: [java]
published: true
excerpt: Conheça o tipo primitivo que representa caractere no Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Tipo caractere (char)

O tipo caractere, como o próprio nome já diz, serve para representar um valor deste tipo. Sua inicialização permite 2 modelos:

{% highlight java %}
char a = ‘a’;
char b = 97; //Equivale a letra ‘a’
{% endhighlight %}

Os caracteres podem ser representados por números e possuem o mesmo tamanho que um atributo do tipo *short*, dessa forma podemos representar a tabela Unicode, exemplo:

{% highlight java %}
char u = ‘\u0025’; //Equivale ao caracter ‘%’
{% endhighlight %}

> O Unicode é no formato hexadecimal, portanto o exemplo anterior **‘0025’** equivale a **37** na base decimal.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d ExemploTipoPrimitivo3.java %}

Quando executamos a classe **ExemploTipoPrimitivo3** temos a seguinte saída no console:

{% highlight java %}
C:\>javac ExemploTipoPrimitivo3.java
C:\>java ExemploTipoPrimitivo3
a
a
%
{% endhighlight %}
