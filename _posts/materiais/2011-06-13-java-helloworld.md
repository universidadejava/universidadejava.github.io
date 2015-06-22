---
layout: article
title: "Java - Hello World"
categories: materiais
author: sakurai
date: 2011-06-13 18:23:00
tags: [java]
published: true
excerpt: Hello World em Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

Durante o desenvolvimento de aplicações utilizando a linguagem Java, precisamos criar arquivos com a extensão .java.

Vamos utilizar o seguinte exemplo, pertencente a um arquivo chamado **PrimeiraClasse.java**:

{% gist f803e0476fb09204b28d %}

Depois de criado este arquivo, acessando a linha de comando iremos executar o seguinte comando para compilar a classe **PrimeiraClasse.java**:

{% highlight java %}
javac PrimeiraClasse.java
{% endhighlight %}

A aplicação **javac** é responsável por compilar o arquivo **.java** gerando o arquivo **.class** de *bytecode*.

Após a execução deste comando, um arquivo com bytecode Java será criado em disco, com o seguinte nome: **PrimeiraClasse.class**.

Um ponto importante da linguagem é que ele é **case sensitive**, ou seja, a letra **‘a’** em minúsculo é diferente da letra **‘A’** em maiúsculo.

Caso escrevemos o código Java, como por exemplo, **“Public”** com **‘P’** maiúsculo ou **“string”** com o **‘s’** minúsculo teremos um erro de compilação e para os iniciantes na linguagem este é um dos maiores problemas encontrados durante a compilação.

Agora para executarmos nosso novo arquivo compilado Java, basta submetê-lo a máquina virtual Java, através do seguinte comando:

{% highlight java %}
java PrimeiraClasse
{% endhighlight %}

Note que, apesar de não estarmos utilizando a extensão, o arquivo submetido foi o arquivo .class.

A aplicação java (utilizada na linha de comando), compõe tanto o pacote da JDK como da JRE.

{% highlight java %}
C:\> javac PrimeiraClasse.java
C:\> java PrimeiraClasse
Hello world !!!
{% endhighlight %}

Quando executamos **java PrimeiraClasse** o Java começa a executar os códigos do nosso programa, nesse caso o método **public static void main(String[] args)** é chamado. O método main é o inicio de tudo, a partir da **main** você pode iniciar seu programa, se preciso pode chamar outras classes, outros métodos, etc.
