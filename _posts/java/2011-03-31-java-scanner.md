---
layout: article
title: "Java - Entrada de dados via console com java.util.Scanner"
categories: java
author: sakurai
date: 2011-03-31 20:34:00
tags: [java]
published: true
excerpt: Leitura de dados do Console usando a classe Scanner.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

Em Java temos uma classe chamada **java.util.Scanner** que neste momento utilizaremos para receber entradas do usuário via console, mas esta classe também é pode ser utilizada para outros fins, tais como leitura de arquivos por exemplo.

No exemplo abaixo vamos utilizar a classe Scanner para pedir que o usuário digite sua idade, depois iremos imprimir qual foi o número lido:

{% gist 484fdcdc3fe2ae8a525d ExemploScanner.java %}

Quando executamos a classe **ExemploScanner**, na linha 11 imprimimos no console a seguinte mensagem:

{% highlight java %}
C:\>javac ExemploScanner.java
C:\>java ExemploScanner
Digite sua idade:
{% endhighlight %}

Na linha 12 o programa fica esperando o usuário digitar um número inteiro e em seguida apertar a tecla **ENTER**, para continuar a execução:

{% highlight java %}
C:\>javac ExemploScanner.java
C:\>java ExemploScanner
Digite sua idade:
27
Vc tem 27 anos.
{% endhighlight %}

Com o Scanner podemos ler diversos tipos de atributos, exemplo:

{% gist 484fdcdc3fe2ae8a525d ExemploScanner2.java %}

Quando executamos a classe **ExemploScanner2** temos a seguinte saída no console:

{% highlight java %}
C:\>javac ExemploScanner2.java
C:\>java ExemploScanner2
Digite seu nome:
Rafael

Digite sua altura:
1,78
Rafael tem 1.78 de altura.
{% endhighlight %}

Fazendo uma comparação com a linguagem C++ os métodos da classe **Scanner nextInt()** (Lê um número inteiro), **nextDouble()** (Lê um número com casa decimal do tipo double), **nextLine()** (Lê um texto “String”), etc. podem ser comparados a função **cin**, e o método **System.out.println()** pode ser comparado a função **cout**.

> Quando queremos ler um número com casa decimal via console, precisamos digitar o numero utilizando vírgula ( , ), exemplo: **10,50**. Quando criamos uma variável dentro do programa e definimos seu valor com casa decimal, precisamos utilizar o ponto ( . ) como separador, exemplo: **10.50**.
