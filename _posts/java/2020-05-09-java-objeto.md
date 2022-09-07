---
layout: article
title: "Objeto"
categories: java
author: sakurai
date: 2020-05-09 14:18:00
tags: [java, objeto, object, new]
published: true
excerpt: Um objeto é a representação (instância) de uma classe.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Objeto

Um objeto é a representação (instância) de uma classe. Vários objetos podem ser criados utilizando-se a mesma classe, mas cada instância pode ter um estado (valor dos atributos) diferentes. Basta pensarmos na Classe como uma grande forma e no Objeto como algo que passou por essa forma, ou seja, o Objeto deve possuir as mesmas características de sua Classe.

Na linguagem Java, para criarmos um novo objeto, basta utilizar a palavra-chave **new** da seguinte forma:

No exemplo a seguir, criamos uma classe nova chamada **TesteNovaClasse**. Esta classe será utilizada para criar um objeto da classe **NovaClasse** e chamar os métodos **metodo1()** e **metodo2()**.

{% highlight java %}
/**
 * Classe utilizada para testar a classe NovaClasse
 */
public class TesteNovaClasse {
  /**
   * Método principal da classe.
   */
  public static void main(String[] args) {
    /* Criando um objeto a partir da classe NovaClasse. */
    NovaClasse novaClasse = new NovaClasse();

    /* Chamando os métodos através da variavel novaClasse. */
    novaClasse.metodo1();
    novaClasse.metodo2();
  }
}
{% endhighlight %}

Na classe acima, criamos uma variável chamada **novaClasse** e esta variável é do tipo **NovaClasse**. Depois utilizamos a palavra-chave **new** seguida de **NovaClasse()** para construirmos um objeto da classe **NovaClasse**.
 
Observe que dentro do método main, estamos utilizando a variável **novaClasse** para invocar os métodos **metodo1()** e **metodo2()**. Para chamar um método ou um atributo a partir de um objeto utilizamos o ponto final ( . ) seguido do nome do método que precisamos chamar.

Quando executamos a classe **TesteNovaClasse**, temos a seguinte saída no console:

{% highlight java %}
C:\>javac NovaClasse.java
C:\>javac TesteNovaClasse.java
C:\>java TesteNovaClasse
Chamando o metodo 1.
Chamando o metodo 2.
{% endhighlight %}

No vídeo a seguir mostramos passo a passo como criar classe e instanciar objetos:

<iframe width="420" height="315" src="https://www.youtube.com/embed/bK-yHMVai_0" frameborder="0" allowfullscreen></iframe>