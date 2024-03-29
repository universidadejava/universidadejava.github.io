---
layout: article
title: "Hello World em Java"
categories: java
author: sakurai
date: 2011-06-13 18:23:00
tags: [java, helloworld]
published: true
excerpt: Hello World em Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

Durante o desenvolvimento de aplicações utilizando a linguagem Java, precisamos criar arquivos com a extensão .java, compilar este arquivo para depois poder executá-lo.

No vídeo a seguir mostramos passo a passo como você pode criar sua primeira classe Java, compilar e executar ela:

<iframe width="420" height="315" src="https://www.youtube.com/embed/xOz0DvJy2cc" frameborder="0" allowfullscreen></iframe>

Vamos praticar mais um pouco? Agora que você já viu como funciona a compilação e execução de uma classe Java, vamos fazer um novo exemplo. Crie um arquivo chamado `PrimeiraClasse.java` com o seguinte conteúdo:

{% gist f803e0476fb09204b28d %}

Depois de criado este arquivo, acessando a linha de comando iremos executar o seguinte comando para compilar a classe `PrimeiraClasse.java`:

{% highlight java %}
javac PrimeiraClasse.java
{% endhighlight %}

A aplicação **javac** é responsável por compilar o arquivo **.java** gerando o arquivo **.class** de *bytecode*.

Após a execução deste comando, um arquivo com bytecode Java será criado em disco, com o seguinte nome: `PrimeiraClasse.class`.

Um ponto importante da linguagem é que ele é **case sensitive**, ou seja, a letra `'a'` em minúsculo é diferente da letra `'A'` em maiúsculo.

Caso escrevemos o código Java, como: `"Public"` com `'P'` maiúsculo ou `"string"` com o `'s'` minúsculo teremos um erro de compilação e para os iniciantes na linguagem este é um dos maiores problemas encontrados durante a compilação.

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

Quando executamos `java PrimeiraClasse` o Java começa a executar os códigos do nosso programa, nesse caso o método `public static void main(String[] args)` é chamado. O método `main` é o inicio de tudo, a partir da `main` você pode iniciar seu programa, se preciso pode chamar outras classes, outros métodos, etc.


### Conteúdos relacionados

- [Criando uma simples classe Java no estilo POJO](http://www.universidadejava.com.br/java/java-pojo/)
- [Conhendo a classe String para manipulação de sequência de caracteres](http://www.universidadejava.com.br/java/java-string/)
- [varargs - Passando uma quantidade variável de parâmetros](http://www.universidadejava.com.br/java/java-varargs/)
- [Introdução a Java Virtual Machine (JVM)](http://www.universidadejava.com.br/java/introducao-jvm/)