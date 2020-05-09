---
layout: article
title: "Java - Casting de tipos primitivos"
categories: java
author: sakurai
date: 2011-06-15 15:48:00
tags: [java]
published: true
excerpt: Veja como funciona conversão de tipos primitivos no Java.
comments: true
image:
  teaser: 2011-06-15-teaser-java-casting-tipos-primitivos.png
ads: true
---

## Casting

Na linguagem Java, é possível se atribuir o valor de um tipo de variável a outro tipo de variável, porém para tal é necessário que esta operação seja apontada ao compilador. A este apontamento damos o nome de *casting*.

É possível fazer conversões de tipos de ponto flutuante para inteiros, e inclusive entre o tipo caractere, porém estas conversões podem ocasionar a perda de valores, quando se molda um tipo de maior tamanho, como um double dentro de um int.

> O tipo de dado boolean é o único tipo primitivo que não suporta casting.

Segue abaixo uma tabela com todos os tipos de casting possíveis:

<figure>
    <a href="/images/2011-06-15-java-casting-tipos-primitivos-01.png"><img src="/images/2011-06-15-java-casting-tipos-primitivos-01.png" alt="Casting de tipos primitivos do Java."></a>
</figure>

Para fazer um casting, basta sinalizar o tipo para o qual se deseja converter entre parênteses, da seguinte forma:

{% highlight java %}
//Conversão do double 5.0 para float.
float a  = (float) 5.0;

//Conversão de double para int.
int b = (int) 5.1987;

//Conversão de int para float é implícito, não precisa de casting.
float c = 100;

//Conversão de char para int é implícito, não precisa de casting.
int d = 'd';
{% endhighlight %}

O casting ocorre implicitamente quando adiciona uma variável de um tipo menor que o tipo que receberá esse valor.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d ExemploCasting.java %}

Quando executamos a classe **ExemploCasting** temos a seguinte saída no console:

{% highlight java %}
C:\>javac ExemploCasting.java
C:\>java ExemploCasting
a
98
100.0
5
5.0
102
n
{% endhighlight %}
