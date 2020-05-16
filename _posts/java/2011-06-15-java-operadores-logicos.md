---
layout: article
title: "Java - Operadores lógicos"
categories: java
author: sakurai
date: 2011-06-15 22:43:00
tags: [java, operadores, logicos, E, OU, negação]
published: true
excerpt: Conheça os operadores lógicos do Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

Os operadores lógicos aceitam apenas operando do tipo boolean.

## Operador E

Símbolo && é chamado de E. Este operador retorna true somente se os dois operandos forem true.

{% highlight java %}
<operando1> && <operando2>
{% endhighlight %}

Se o valor do operando1 for false, então o operador && não verifica o valor do operador2, pois sabe que o resultado já é false.

Tabela verdade:

Operando1 | Operando2 | Resultado
--------- | --------- | ---------
true | true | true
true | false | false
false | true | false
false | false | false

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorLogicoE.java %}

Neste caso será impresso false, pois a variável a é true e a variável b é false, depois será impresso true, pois ambas variáveis a e c são true.

{% highlight java %}
C:\>javac OperadorLogicoE.java
C:\>java OperadorLogicoE
false
true
{% endhighlight %}

## Operador OU

Símbolo \|\| é chamado de OU. Este operado retorna true caso tenha pelo menos um operando com o valor true.

{% highlight java %}
<operando1> || <operando2>
{% endhighlight %}

Se o valor do operando1 for true, então o operador \|\| não verifica o valor do operando2, pois já sabe que o resultado é true.

Tabela verdade:

Operando1 | Operando2 | Resultado
--------- | --------- | ---------
true | true | true
true | false | true
false | true | true
false | false | false

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorLogicoOU.java %}

Neste caso será impresso primeiro true, pois a variável a tem o valor true, depois será impresso false, pois as variáveis b e c são false.

{% highlight java %}
C:\>javac OperadorLogicoOU.java
C:\>java OperadorLogicoOU
true
false
{% endhighlight %}

## Operador de negação

Símbolo ! é chamado de negação. Este operador retorna true se o operando tem o valor false, e retorna false se o operando o valor true.

{% highlight java %}
! <operando>
{% endhighlight %}

Tabela verdade:

Operando | Resultado
-------- | ---------
false | true
true | false

Exemplo:

{% gist 484fdcdc3fe2ae8a525d OperadorLogicoNegacao.java %}

Neste caso primeiro será impresso false, que é a negação de true, depois será impresso true, que é a negação do resultado de b \|\| c que é false.

{% highlight java %}
C:\>javac OperadorLogicoNegacao.java
C:\>javac OperadorLogicoNegacao
false
true
{% endhighlight %}
