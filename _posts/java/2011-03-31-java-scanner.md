---
layout: article
title: "Entrada de dados via console com java.util.Scanner"
categories: java
author: sakurai
date: 2011-03-31 20:34:00
tags: [java, input, console, scanner]
published: true
excerpt: Leitura de dados do Console usando a classe Scanner.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

Em Java temos a classe `java.util.Scanner` que neste momento utilizaremos para receber entradas do usuário via console, mas esta classe também pode ser utilizada para outros fins, como [leitura de arquivos](http://www.universidadejava.com.br/java/java-leitura-arquivo/).

No exemplo a seguir vamos utilizar a classe Scanner para pedir que o usuário digite sua idade, depois iremos imprimir qual foi o número lido:

{% gist 484fdcdc3fe2ae8a525d ExemploScanner.java %}

Quando executamos a classe `ExemploScanner`, na linha 11 imprimimos no console a seguinte mensagem:

{% highlight java %}
C:\>javac ExemploScanner.java
C:\>java ExemploScanner
Digite sua idade:
{% endhighlight %}

Na linha 12 o programa fica esperando o usuário digitar um número inteiro e em seguida apertar a tecla `ENTER`, para continuar a execução:

{% highlight java %}
C:\>javac ExemploScanner.java
C:\>java ExemploScanner
Digite sua idade:
27
Vc tem 27 anos.
{% endhighlight %}

Com o Scanner podemos ler diversos tipos de atributos, exemplo:

{% gist 484fdcdc3fe2ae8a525d ExemploScanner2.java %}

Quando executamos a classe `ExemploScanner2` temos a seguinte saída no console:

{% highlight java %}
C:\>javac ExemploScanner2.java
C:\>java ExemploScanner2
Digite seu nome:
Rafael

Digite sua altura:
1,78
Rafael tem 1.78 de altura.
{% endhighlight %}

Fazendo uma comparação com a linguagem C++ os métodos da classe `Scanner` `nextInt()` (lê um número [inteiro](http://www.universidadejava.com.br/java/java-tipo-numerico-inteiro/)), `nextDouble()` (lê um número com casa decimal do tipo [double](http://www.universidadejava.com.br/java/java-tipo-numerico-ponto-flutuante/)), `nextLine()` (lê um texto [String](http://www.universidadejava.com.br/java/java-string/)), etc. podem ser comparados a função `cin`, e o método `System.out.println()` pode ser comparado a função `cout`.

> Observação: quando queremos ler um [número com casa decimal](http://www.universidadejava.com.br/java/java-tipo-numerico-ponto-flutuante/) via console, precisamos digitar o numero utilizando vírgula ( , ), exemplo: `10,50`. Quando criamos uma variável dentro do programa e definimos seu valor com casa decimal, precisamos utilizar o ponto ( . ) como separador, exemplo: `10.50`.

Quer praticar mais um pouco? No vídeo a seguir mostramos mais um exemplo passo a passo de como podemos usar a classe `Scanner`:

<iframe width="420" height="315" src="https://www.youtube.com/embed/nXJnc5kP1EM" frameborder="0" allowfullscreen></iframe>


### Conteúdos relacionados

- [Conversão (casting) de tipos primitivos](http://www.universidadejava.com.br/java/java-casting-tipos-primitivos/)
- [Exercícios com operadores e tipos primitivos](http://www.universidadejava.com.br/java/java-exercicios-tipos-primitivos/)
- [Leitura de arquivos em Java](http://www.universidadejava.com.br/java/java-leitura-arquivo/)
- [Tratando exceções no programa Java](http://www.universidadejava.com.br/java/java-excecoes/)