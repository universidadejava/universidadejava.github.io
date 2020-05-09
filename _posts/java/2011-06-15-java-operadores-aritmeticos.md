---
layout: article
title: "Java - Operadores aritméticos"
categories: java
author: sakurai
date: 2011-06-15 18:12:00
tags: [java]
published: true
excerpt: Conheça os operadores aritméticos do Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

##Operador de adição

Símbolo + é chamado de adição, utilizado para somar o valor de dois operandos.

{% highlight java %}
<operando1> + <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorAdicao.java %}

Neste caso será impresso o valor 10 que é o resultado da soma de 3 + 7, ou da soma da variável a + variável b.

{% highlight java %}
C:\>javac OperadorAdicao.java
C:\>java OperacaoAdicao
10
10
{% endhighlight %}

##Operador de subtração

Símbolo - é chamado de subtração, utilizado para subtrair o valor de dois operandos.

{% highlight java %}
<operando1> - <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorSubtracao.java %}

Neste caso será impresso o valor 3 que é o resultado da subtração de 5 – 2, ou da subtração da variável a – variável b.

{% highlight java %}
C:\>javac OperadorSubtracao.java
C:\>java OperadorSubtracao
3
3
{% endhighlight %}

##Operador de multiplicação

Símbolo * é chamado de multiplicação, utilizado para multiplicar o valor de dois operandos.

{% highlight java %}
<operando1> * <operando2>
{% endhighlight %}

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorMultiplicacao.java %}

Neste caso será impresso o valor 6 que é o resultado da multiplicação de 3 * 2, ou da multiplicação da variável a * variável b.

{% highlight java %}
C:\>javac OperadorMultiplicacao.java
C:\>java OperadorMultiplicacao
6
6
{% endhighlight %}

##Operador de divisão

Símbolo / é chamado de divisão, utilizado para dividir o valor de dois operandos.

{% highlight java %}
<operando1> / <operando2>
{% endhighlight %}

O resultado da divisão de dois operandos inteiros retorna um valor inteiro, e a divisão tendo pelo menos um operando com casas decimais retorna um valor com casas decimais.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorDivisao.java %}

Neste caso será impresso o valor 1.5 que é o resultado da divisão de 3.0 / 2, pois o operando 3.0 é do tipo double.

Depois será impresso o valor 1 que é o resultado da divisão da variável a / variável b, note que neste caso ambos são inteiros então o resultado da divisão é um valor inteiro.

{% highlight java %}
C:\>javac OperadorDivisao.java
C:\>java OperadorDivisao
1.5
1
{% endhighlight %}

##Operador de módulo

Símbolo % é chamado de módulo, utilizado para saber qual o resto da divisão de dois operandos.

{% highlight java %}
<operando1> % <operando2>
{% endhighlight %}

O resto da divisão de dois operandos inteiros retorna um valor inteiro, e o resto da divisão tendo pelo menos um operando com casas decimais retorna um valor com casas decimais.

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorModulo.java %}

Neste caso será impresso o valor 0.5 que é o resto da divisão de 4.5 / 2, note que neste caso um operando possui casa decimal.

Depois será impresso o valor 2 que é o resto da divisão da variável a / variável b, note que neste caso ambos são inteiros então o resultado da divisão é um valor inteiro.

{% highlight java %}
C:\>javac OperadorModulo.java
C:\>java OperadorModulo
0.5
2
{% endhighlight %}
