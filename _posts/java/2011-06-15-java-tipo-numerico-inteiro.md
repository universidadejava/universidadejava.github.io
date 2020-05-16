---
layout: article
title: "Java - Tipo primitivo numérico"
categories: java
author: sakurai
date: 2011-06-15 15:26:00
tags: [java, tipos, primitivos, byte, short, int, long]
published: true
excerpt: Conheça os tipos primitivos que representam números inteiros no Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Tipos Inteiros (byte, short, int ou long)

Tipos inteiros trabalham apenas com números inteiros, positivos ou negativos. Os valores podem ser representados nas bases *octal*, *decimal* e *hexadecimal*.

### Inteiro em octal
Qualquer valor escrito utilizando números de 0 a 7 começando com 0 é um valor na base octal, exemplo:

{% highlight java %}
byte a = 011;        // Equivale ao valor 9 em decimal.
short s = 010;       // Equivale ao valor 8 em decimal.
int i = 025;         // Equivale ao valor 21 em decimal.
{% endhighlight %}

### Inteiro em decimal
Qualquer valor escrito utilizando números de 0 a 9 é um valor decimal, este é o tipo de representação mais comum uma vez que é utilizada no dia a dia, exemplo:

{% highlight java %}
int i = 9;
long b = 9871342132;
{% endhighlight %}

### Inteiro em hexadecimal
Qualquer valor escrito utilizando números de 0 a 9 e A a F começando com 0x ou 0X é um valor hexadecimal, exemplo:

{% highlight java %}
long a = 0xCAFE;    // Equivale ao valor 51966 em decimal
int b = 0X14a3;     // Equivale ao valor 5283 em decimal
{% endhighlight %}

Quando um precisa ser especificado como um long ele pode vir acompanhado por l ou L depois do seu valor, exemplo:

{% highlight java %}
long a = 0Xcafel;
long b = 0752L;
long c = 987654321L;
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d ExemploTipoPrimitivo.java %}

Quando executamos a classe ExemploTipoPrimitivo temos a seguinte saída no console:

{% highlight java %}
C:\>javac ExemploTipoPrimitivo.java
C:\>java ExemploTipoPrimitivo
63
8
21
9
9871342132
51966
51966
490
987654321
{% endhighlight %}
