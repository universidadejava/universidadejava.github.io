---
layout: article
title: "Operadores de atribuição"
categories: java
author: sakurai
date: 2011-06-15 18:42:00
tags: [java, operadores, atribuição]
published: true
excerpt: Conheça os operadores aritméticos do Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

##Operador de atribuição

Símbolo = é chamado de atribuição, utilizado para atribuir o valor de um operando a uma variável:

{% highlight java %}
<operando1> = <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicao.java %}

Neste caso a variável x possui agora o valor 25.

{% highlight java %}
C:\>javac OperadorAtribuicao.java
C:\>java OperadorAtribuicao
25
{% endhighlight %}

A operação de atribuição também pode receber como operando o resultado de outra operação, exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicao2.java %}

Neste caso a variável x possui o valor 0 que é o resto da divisão de 4 por 2.

{% highlight java %}
C:\>javac OperadorAtribuicao2.java
C:\>java OperadorAtribuicao
0
{% endhighlight %}

##Juntando operador de atribuição com operadores aritméticos

Operador de atribuição com adição

Símbolo += é utilizado para atribuir a uma variável o valor desta variável somada ao valor de um operando.

{% highlight java %}
<operando1> += <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicaoAdicao.java %}

Neste caso a variável x começa com o valor 4, depois a variável x recebe o valor dela somado ao valor 2, portanto a variável x fica com o valor 6.

{% highlight java %}
C:\>javac OperadorAtribuicaoAdicao.java
C:\>java OperadorAtribuicaoAdicao
6
{% endhighlight %}

##Operador de atribuição com subtração

Símbolo -= é utilizado para atribuir a uma variável o valor desta variável subtraindo o valor de um operando.

{% highlight java %}
<operando1> -= <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicaoSubtracao.java %}

Neste caso a variável x começa com o valor 4, depois a variável x recebe o valor dela subtraído pelo valor 2, portanto a variável x fica com o valor 2.

{% highlight java %}
C:\>javac OperadorAtribuicaoSubtracao.java
C:\>java OperadorAtribuicaoSubtracao
2
{% endhighlight %}

##Operador de atribuição com multiplicação

Símbolo \*= é utilizado para atribuir a uma variável o valor desta variável multiplicado com o valor de um operando.

{% highlight java %}
<operando1> *= <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicaoMultiplicacao.java %}

Neste caso a variável x começa com o valor 3, depois a variável x recebe o valor dela multiplicado pelo valor 5, portanto a variável x fica com o valor 15.

{% highlight java %}
C:\>javac OperadorAtribuicaoMultiplicacao.java
C:\>java OperadorAtribuicaoMultiplicacao
15
{% endhighlight %}

##Operador de atribuição com divisão

Símbolo /= é utilizado para atribuir a uma variável o valor desta variável dividido pelo valor de um operando.

{% highlight java %}
<operando1> /= <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicaoDivisao.java %}

Neste caso a variável x começa com o valor 4, depois a variável x recebe o valor dela dividido pelo valor 3, portanto a variável x fica com o valor 1.

{% highlight java %}
C:\>javac OperadorAtribuicaoDivisao.java
C:\>java OperadorAtribuicaoDivisao
1
{% endhighlight %}

Quando usamos o operador /= utilizando uma variável inteira e um operando de casa decimal, então a divisão retorna um valor inteiro.

Caso utilize uma variável de ponto flutuante, então a divisão retorna um valor com casa decimal, exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicaoDivisao2.java %}

Neste caso a variável x terá o valor 1 impresso e a variável y terá o valor 1.3333334 impresso.

{% highlight java %}
C:\>javac OperadorAtribuicaoDivisao2.java
C:\>java OperadorAtribuicaoDivisao2
1
1.3333334
{% endhighlight %}

##Operador de atribuição com módulo

Símbolo %= é utilizado para atribuir a uma variável, o valor do resto da divisão desta variável por um operando.

{% highlight java %}
<operando1> %= <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicaoModulo.java %}

Neste caso a variável x começa com o valor 4, depois a variável x recebe o resto da divisão dela pelo valor 3, portanto a variável x fica com o valor 1.

{% highlight java %}
C:\>javac OperadorAtribuicaoModulo.java
C:\>java OperadorAtribuicaoModulo
1
{% endhighlight %}

Quando usamos o operador %= utilizando uma variável inteira e um operando de casa decimal, então o resto da divisão retorna um valor inteiro.

Caso utilize uma variável de ponto flutuante, então o resto da divisão retorna um valor com casa decimal, exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAtribuicaoModulo2.java %}

Neste caso a variável x terá o valor 1 impresso e a variável y terá o valor 0.67 impresso.

{% highlight java %}
C:\>javac OperadorAtribuicaoModulo2.java
C:\>java OperadorAtribuicaoModulo2
1
0.67
{% endhighlight %}
