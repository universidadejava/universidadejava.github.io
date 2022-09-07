---
layout: article
title: "Construtor"
categories: java
author: sakurai
date: 2020-05-09 14:32:00
tags: [java, construtor, new]
published: true
excerpt: Construtor.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Construtor

Sempre quando criamos um novo objeto em Java, utilizamos à sintaxe:

{% highlight java %}
Classe nomeObjeto = new Classe();
{% endhighlight %}

O que ainda não foi comentado sobre este comando é a necessidade de se referenciar o método construtor daquela classe. Um método construtor, como o próprio nome já diz, é responsável pela criação do objeto daquela classe, iniciando com valores seus atributos ou realizando outras funções que possam vir a ser necessárias. Para que um método seja considerado **construtor**, ele **deve possuir o mesmo nome da classe, inclusive com correspondência entre letras maiúsculas e minúsculas e não deve ter retorno**.

Quando usamos a palavra-chave **new**, estamos passando para ela como um parâmetro, qual construtor deve ser executado para instanciar um objeto.

Por padrão, todas as classes possuem um construtor com seu nome seguindo de parênteses “()”. Caso você não declare manualmente o construtor, o compilador do Java fará isso por você.

Vamos criar um exemplo de construtor padrão que não recebe:

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso do construtor.
 */
public class ClasseConstrutor {
  /**
   * Construtor padrão.
   *
   * Note que o construtor possui o mesmo nome da classe e não informa o
   * retorno.
   */
  public ClasseConstrutor() {
    System.out.println("Criando um objeto da classe ClasseConstrutor.");
  }
}
{% endhighlight %}

Neste exemplo, sempre que criarmos um objeto da classe **ClasseConstrutor**, a frase **“Criando um objeto da classe ClasseConstrutor.”** será impressa no console.

No exemplo a seguir, criaremos um construtor que recebe parâmetros:

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso do construtor
 * que inicializa os atributos da classe. 
 */
public class ClasseConstrutor2 {
  public int atributo1;
  public float atributo2;
  public boolean atributo3;

  /**
   * Construtor que recebe os valores para inicializar os atributos.
   * 
   * @param valor1 - Valor inteiro que será guardado no atributo1.
   * @param valor2 - Valor float que será guardado no atributo2.
   * @param valor3 - Valor boolean que será guardado no atributo3.
   */
  public ClasseConstrutor2(int valor1, float valor2, boolean valor3) {
    System.out.println("Criando um objeto da classe ClasseConstrutor2.");
    System.out.println("Recebeu os seguintes parametros:\n\t" + valor1);
    System.out.println("\t" + valor2);
    System.out.println("\t" + valor3);

    atributo1 = valor1;
    atributo2 = valor2;
    atributo3 = valor3;
  }
}
{% endhighlight %}

Neste exemplo, para construirmos um objeto da classe **ClasseConstrutor2**, é necessário passar para o construtor três parâmetros: **int, float e boolean**. Se não passarmos todos os parâmetros ou a ordem deles estiver diferente do esperado não conseguiremos compilar a classe.

**Quando criamos um construtor que recebe parâmetros, o compilador não cria um construtor padrão ClasseConstrutor2().**

No exemplo a seguir, vamos construir um objeto da classe **ClasseConstrutor** e um objeto da classe **ClasseConstrutor2**.

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso do construtor.
 */
public class TesteConstrutor {
  /**
   * Método principal que cria dois objetos.
   */
  public static void main(String[] args) {
    /* Chama o construtor padrão da classe ClasseConstrutor. */
    ClasseConstrutor cc = new ClasseConstrutor();
    /* Chama o construtor da classe ClasseConstrutor2 passando
       os valores que serão guardados nos atributos. */
    ClasseConstrutor2 cc2 = new ClasseConstrutor2(10, 3.5F, false);
  }
}
{% endhighlight %}

Criamos um objeto da classe **ClasseConstrutor** chamado o construtor sem parâmetros **ClasseConstrutor()**.

Em seguida, criamos um objeto da classe **ClasseConstrutor2**, chamando o construtor **ClasseConstrutor2(int valor1, float valor2, boolean valor3)**, passando os três parâmetros para ele.

Quando executamos a classe **TesteConstrutor**, temos a seguinte saída no console:

{% highlight java %}
C:\>javac ClasseConstrutor.java
C:\>javac ClasseConstrutor2.java
C:\>javac TesteConstrutor.java
C:\>java TesteConstrutor
Criando um objeto da classe ClasseConstrutor.
Criando um objeto da classe ClasseConstrutor2.
Recebeu os seguintes parametros:
	10
	3.5
	false
{% endhighlight %}

No vídeo a seguir mostramos passo a passo como criar e usar o construtor da classe:

<iframe width="420" height="315" src="https://www.youtube.com/embed/JEmvQo9W5l0" frameborder="0" allowfullscreen></iframe>