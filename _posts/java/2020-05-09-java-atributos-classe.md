---
layout: article
title: "Java - atributos da classe"
categories: java
author: sakurai
date: 2020-05-09 14:23:00
tags: [java]
published: true
excerpt: Utilizando os atributos da classe.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Utilizando os atributos da classe

No exemplo a seguir, criamos a classe **Atributo** e dentro dela declaramos três atributos diferentes:

{% highlight java %}
/**
 * Classe utilizada para demonstrar a utilização de
 * atributos.
 */
public class Atributo {
  /* Declaração dos atributos da classe. */
  public int atributo1;
  public float atributo2;
  public boolean atributo3;
}
{% endhighlight %}

Criamos um atributo para guardar um valor inteiro do tipo **int** com o nome **atributo1**. Por enquanto utilize a palavra-chave **public** na declaração do atributo.

Criamos um atributo para guardar um valor com ponto flutuante do tipo **float** com o nome **atributo2**.

Criamos um atributo para guardar um valor **booleano** chamado **atributo3**.

No exemplo a seguir, criaremos um objeto da classe **Atributo** e atribuiremos valores para as variáveis. Depois imprimiremos os seus valores:

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso
 * dos atributos de outra classe.
 */
public class TesteAtributo {
  /**
   * Método principal para testar a classe Atributo.
   */
  public static void main(String[] args) {
    System.out.println("Cria um objeto da classe Atributo.");
    Atributo teste = new Atributo();
    teste.atributo1 = 30;
    teste.atributo2 = 3.5f;
    teste.atributo3 = false;

    System.out.println("Valor do atributo1: " + teste.atributo1);
    System.out.println("Valor do atributo2: " + teste.atributo2);
    System.out.println("Valor do atributo3: " + teste.atributo3);
  }
}
{% endhighlight %}

Quando executamos a classe **TesteAtributo**, temos a seguinte saída no console:

{% highlight java %}
C:\>javac Atributo.java
C:\>javac TesteAtributo.java
C:\>java TesteAtributo
Cria um objeto da classe Atributo.
Valor do atributo1: 30
Valor do atributo2: 3.5
Valor do atributo3: false
{% endhighlight %}