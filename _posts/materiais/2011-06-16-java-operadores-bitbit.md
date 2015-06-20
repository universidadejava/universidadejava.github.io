---
layout: article
title: "Java - Operadores bit a bit"
categories: materiais
author: sakurai
date: 2011-06-16 21:40:00
tags: [java]
published: true
excerpt: Conheça os operadores bit a bit do Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Operador E bit a bit

Estes operadores são chamados de bit a bit, porque os operando são comparados no nível dos seus bits.

Símbolo & é chamado de E bit a bit.

{% highlight java %}
<operando1> & <operando2>
{% endhighlight %}

Para este operador não importa se algum dos operando tem o valor false, ele vai verificar todos os operandos que houver na expressão.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorBitBitE.java %}

Neste caso será impresso o valor false, e o operador & ira comparar o valor de todas variáveis.

{% highlight java %}
C:\>javac OperadorBitBitE.java
C:\>java OperadorBitBit
false
{% endhighlight %}

Se usarmos valores inteiros, ele irá comparar cada bit, exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorBitBitE2.java %}

Neste caso será impresso o valor 6, porque o operador & vai comparar cada bit na base binária e depois a mesma é convertida para a base decimal.

{% highlight java %}
C:\>javac OperadorBitBitE2.java
C:\>java OperadorBitBit
6
{% endhighlight %}

Lembrando que um inteiro tem 16 bits, então:

Variável a tem o valor 30.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 0

Variável b tem o valor 7.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1

Resultado tem o valor 6.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0

Tabela verdade:

Operando1 | Operando2 | Resultado
--------- | --------- | ---------
1 | 1 | 1
1 | 0 | 0
0 | 1 | 0
0 | 0 | 0

## Operador OU bit a bit

Símbolo \| é chamado de OU bit a bit.

{% highlight java %}
<operando1> | <operando2>
{% endhighlight %}

Para este operador não importa se algum dos operando tem o valor true, ele vai verificar todos os operando que tiver na expressão.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorBitBitOU.java %}

Neste caso o valor impresso será true, e o operador | ira comparar o valor de todas variáveis.

{% highlight java %}
C:\>javac OperadorBitBitOU.java
C:\>java OperadorBitBitOU
true
{% endhighlight %}

Se usarmos valores inteiros, ele irá comparar cada bit, exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorBitBitOU2.java %}

Neste caso será impresso o valor 31, porque o operador \| vai comparar cada bit na base binária e depois a mesma é convertida para a base decimal.

{% highlight java %}
C:\>javac OperadorBitBitOU2.java
C:\>java OperadorBitBitOU2
true
{% endhighlight %}

Lembrando que um inteiro tem 16 bits, então:

Variável a tem o valor 30.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 0

Variável b tem o valor 7.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1

Resultado tem o valor 31.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 1

Tabela verdade:

Operando1 | Operando2 | Resultado
--------- | --------- | ---------
1 | 1 | 1
1 | 0 | 1
0 | 1 | 1
0 | 0 | 0

## Operador OU EXCLUSIVO bit a bit

Símbolo ^ é chamado de OU EXCLUSIVO bit a bit.

{% highlight java %}
<operando1> ^ <operando2>
{% endhighlight %}

Se usarmos valores inteiros, por exemplo ele ira comparar cada bit, exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorBitBitOUExclusivo.java %}

Neste caso será impresso o valor 25, porque o operador ^ vai comparar cada bit na base binária e depois a mesma é convertida para a base decimal.

{% highlight java %}
C:\>javac OperadorBitBitOUExclusivo.java
C:\>java OperadorBitBitOUExclusivo
25
{% endhighlight %}

Lembrando que um inteiro tem 16 bits, então:

Variável a tem o valor 30.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 0

Variável b tem o valor 7.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1

Resultado tem o valor 25.

0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 1

Tabela verdade:

Operando1 | Operando2 | Resultado
--------- | --------- | ---------
1 | 1 | 0
1 | 0 | 1
0 | 1 | 1
0 | 0 | 0
