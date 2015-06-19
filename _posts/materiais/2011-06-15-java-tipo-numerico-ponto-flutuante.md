---
layout: article
title: "Java - Tipos primitivos com ponto flutuante"
categories: materiais
author: sakurai
date: 2011-06-15 15:26:00
tags: [java]
published: true
excerpt: Conheça os tipos primitivos que representam números com ponto flutuante no Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Tipos Ponto Flutuante (float ou double)

Tipos de ponto flutuante serve para representar números com casas decimais, tanto negativos quanto positivos. Todos números com ponto flutuante são por padrão do tipo double, mas é possível especificar o tipo do valor durante a criação, para float utilize **f** ou **F** e se quiser pode especificar para double usando **d** ou **D**, exemplo:

{% highlight java %}
float a = 10.99f;
double b = 10.3D;

floaf c = 1.99; // Erro de compilação pois o padrão do valor é double.
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d ExemploTipoPrimitivo2.java %}

Quando executamos a classe **ExemploTipoPrimitivo2** temos a seguinte saída no console:

{% highlight java %}
C:\>javac ExemploTipoPrimitivo2.java
C:\>java ExemploTipoPrimitivo2
10.99
10.3
5.0
7.2
{% endhighlight %}
