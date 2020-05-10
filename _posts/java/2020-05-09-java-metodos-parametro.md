---
layout: article
title: "Java - métodos com parâmetro"
categories: java
author: sakurai
date: 2020-05-09 14:32:00
tags: [java]
published: true
excerpt: Utilizando métodos com parâmetro.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Métodos com recebimento de parâmetro

Na linguagem Java, os métodos também são capazes de receber um ou mais parâmetros que são utilizados no processamento do método.

No exemplo a seguir, criamos dois métodos que recebem parâmetros e os utilizam no processamento do método:

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso de
 * métodos que recebem parametros.
 */
public class MetodoParametro {
  /* Declaração dos atributos da classe. */
  public int atributo1;

  /* Declaração dos métodos da classe. */

  /**
   * Método utilizado para atribuir o valor do atributo1.
   */
  public void metodo1(int valor) {
    System.out.println("Chamando o metodo 1.");
    atributo1 = valor;
    System.out.println("O valor do atributo1 eh: " + atributo1);
  }

  /**
   * Método que recebe uma quantidade de parametros variados
   * e imprime todos os valores recebidos.
   * Essa possibilidade de receber uma quantidade de parametros
   * variados é chamado de varargs e foi implementado a partir
   * da versão 5.0 do java.
   */
  public void metodo2(int... valores) {
      System.out.println("Chamando o método 2.");
    /* Verifica se recebeu algum argumento. */
    if(valores.length > 0) {
      /* Para cada argumento recebido como parametro, imprime seu valor. */
      for(int cont = 0; cont < valores.length; cont++) {
        int valor = valores[cont];
        System.out.print(valor + " ");
      }
      System.out.println("\n");

      /* Este for faz a mesma coisa que o anterior, este novo tipo de for
         chamado foreach foi implementado a partir da versão 5.0 do java. */
      for(int valor : valores) {
        System.out.print(valor + " ");
      }
      System.out.println("\n");
    }
  }
}
{% endhighlight %}

Na linha 14 o **metodo1(int valor)** recebe um parâmetro inteiro do tipo **int** chamado **valor**, e dentro do método podemos utilizar este atributo **valor**.

Note que a declaração de um parâmetro é igual à declaração de um atributo na classe, informamos seu **tipo** e **identificador**.

Se necessário, podemos declarar tantos parâmetros quantos forem precisos para execução do método. Se o parâmetro recebido no método for primitivo, então seu valor é recebido por cópia, caso receba um objeto como parâmetro seu valor é recebido por referência.

Quando fazemos uma chamada a um método com parâmetros de entrada, um erro na passagem dos tipos dos parâmetros representa um erro de compilação.

Quando é necessário passar uma quantidade de parâmetros muito grande ou uma quantidade desconhecida de parâmetros, isso pode ser feito através de um **array** ou podemos usar **varargs**.

A sintaxe do **varargs** é:

{% highlight java %}
tipo... identificador
{% endhighlight %}

O método **metodo2(int... valores)** recebe uma quantidade variável de valores inteiros do tipo **int**.

A seguir criaremos um objeto da classe **MetodoParametro** e vamos utilizar o **metodo1(int valor)** e **metodo2(int... valores)**.

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso
 * da chamada de métodos de outra classe.
 */
public class TesteMetodoParametro {
  /**
   * Método principal para testar a classe MetodoParametro.
   */
  public static void main(String[] args) {
    System.out.println("Cria um objeto da classe MetodoParametro.");
    MetodoParametro teste = new MetodoParametro();
    teste.metodo1(100);

    /* Chama o método sem passar parametro. */
    teste.metodo2();
    /* Chama o método passando um parametro. */
    teste.metodo2(10);
    /* Chama o método passando dez parametros. */
    teste.metodo2(10, 20, 30, 40, 50, 60, 70, 80, 90, 100);
  }
}
{% endhighlight %}

Criamos um atributo do tipo **MetodoParametro** chamado **teste**, e depois criamos um objeto da classe **MetodoParametro**.

A partir do objeto teste chamamos o **metodo1** passando o valor **100** como parâmetro.

Depois, chamamos o **metodo2**, passando valores variados para ele.

Quando executamos a classe **TesteMetodoParametro**, temos a seguinte saída no console:

{% highlight java %}
C:\>javac MetodoParametro.java
C:\>javac TesteMetodoParametro.java
C:\>java TesteMetodoParametro
Cria um objeto da classe MetodoParametro.
Chamando o metodo 1.
O valor do atributo eh: 100
Chamando o metodo 2.
Chamando o metodo 2.
10

10
Chamando o metodo 2.
10 20 30 40 50 60 70 80 90 100

10 20 30 40 50 60 70 80 90 100
{% endhighlight %}