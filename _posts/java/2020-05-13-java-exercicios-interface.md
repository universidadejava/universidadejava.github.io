---
layout: article
title: "Exercícios sobre interface"
categories: java
author: sakurai
date: 2020-05-10 00:17:00
tags: [java, interface, exercícios]
published: true
excerpt: Exercícios para praticar o uso de interfaces.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Exercícios sobre interfaces

1. Vamos criar um programa Java para ordenar vários tipos de objeto diferente, utilizando o algoritmo de Bubble Sort.

Para isto vamos utilizar uma interface chamada **Comparavel**. Esta interface irá possuir a assinatura de método que todas as classes que desejam ser comparadas precisa implementar:

{% highlight java %}
package br.universidadejava.exercicioInterface;

/**
 * Interface que deve ser implementada por todas as classes que 
 * devem ser ordenadas.
 */
public interface Comparavel {

  /**
   * Assinatura de método que toda classe que quer permitir
   * a comparação entre seus objetos precisa implementar.
   * 
   * @param o - Objeto que será comparado.
   * @return 0 se os objetos forem iguais.
   *         > 0 se o objeto recebido é menor que o objeto que será comparado.
   *         < 0 se o objeto recebido é maior que o objeto que será comparado.
   */
  public abstract int comparar(Object o);
}
{% endhighlight %}

Vamos criar uma classe que ordena um **array** de objetos que são do tipo **Comparavel**:

{% highlight java %}
package br.universidadejava.exercicioInterface;

/**
 * Classe utilizada para ordenar qualquer tipo de classe
 * que implementa a interface Comparavel.
 */
public class Ordenar {
  /**
   * Método que utiliza o algoritmo de bubble sort
   * para ordenar um vector de objetos do tipo <code>Comparavel</code>.
   * 
   * @param objetos - Vetor de objetos que serão ordenados.
   */
  public void ordenar(Comparavel[] objetos) {
    for(int i = 0; i < objetos.length; i++) {
      for(int j = i + 1; j < objetos.length; j++) {
        /* Verifica se os objetos não estão na ordem. */
        if(objetos[i].comparar(objetos[j]) > 0) {
          /* Troca os objetos de lugar no vetor. */
          Comparavel temp = objetos[i];
          objetos[i] = objetos[j];
          objetos[j] = temp;
        }
      }
    }
  }
}
{% endhighlight %}

Como todos os objetos que são comparáveis implementam a interface Comparavel, então criamos o método ordenar, que recebe um vetor de objetos do tipo Comparavel, e como uma classe implementa os métodos da interface, sabemos que todas as classes comparáveis terão o método comparar, por isso podemos ordenar os objetos usando o método **objeto[i].comparar(objeto[j])**.

Agora, vamos criar uma classe que implemente a interface **Comparavel**. Quando uma classe implementa uma interface, esta classe, obrigatoriamente, precisa implementar todos os métodos declarados na interface. Apenas quando usamos classes abstratas implementando interface é que não precisamos, obrigatoriamente, implementar todos os métodos.

{% highlight java %}
package br.universidadejava.exercicioInterface;

/**
 * Classe utilizada para representar um Livro, está classe
 * implementa a interface Comparavel.
 */
public class Livro implements Comparavel {
  private String autor;
  private String titulo;

  public Livro(String autor, String titulo) {
    this.autor = autor;
    this.titulo = titulo;
  }

  public String getAutor() {
    return autor;
  }

  public void setAutor(String autor) {
    this.autor = autor;
  }

  public String getTitulo() {
    return titulo;
  }

  public void setTitulo(String titulo) {
    this.titulo = titulo;
  }

  public int comparar(Object o) {
    int comparacao = 0;

    //Verifica se o objeto que vai comparar é do tipo Livro.
    if(o instance of Livro) {
      Livro livro = (Livro) o;
      comparacao = this.getAutor().compareTo(livro.getAutor());
      
      //Se os autores forem iguais, compara o titulo dos livros.
      if(comparacao == 0) {
        comparacao = this.getTitulo().compareTo(livro.getTitulo());
      }
    }
    return comparacao;
  }
}
{% endhighlight %}

A classe livro implementou o método **comparar** para fazer a comparação de acordo com o autor e título do livro.

Agora, vamos criar uma classe para testar a ordenação de um vetor de Livros.

{% highlight java %}
package br.universidadejava.exercicioInterface;

/**
 * Classe utilizada para testar a ordenação generica.
 */
public class TestarOrdenacao {
  public static void main(String[] args) {
    /* Cria um vetor de livros. */
    Livro[] livros = new Livro[4];
    livros[0] = new Livro("Sakurai", "Almoçando com Java");
    livros[1] = new Livro("Cristiano", "Classes Java em fila indiana");
    livros[2] = new Livro("Sakurai", "Java em todo lugar");
    livros[3] = new Livro("Cristiano", "Viajando no Java");

    /* Ordena os livros */
    Ordenar o = new Ordenar();
    o.ordenar(livros);

    /* Imprime os livros ordenados. */
    for(int cont = 0; cont < livros.length; cont++) {
      System.out.println("Autor: " + livros[cont].getAutor());
      System.out.println("Livro: " + livros[cont].getTitulo());
      System.out.println("\n--------\n");
    }
  }
}
{% endhighlight %}

Quando chamamos **o.ordenar(livros)**, estamos passando o vetor de objetos livros para ser ordenado. Como Livro implementa Comparavel podemos passar um objeto do tipo Livro em que é esperado um objeto do tipo Comparavel. O método ordenar() irá chamar o método comparar() que foi implementado pela classe Livro, isto é feito em tempo de execução da aplicação, a JVM verifica qual o tipo da classe do objeto e chama o método que foi implementado nele, isto é chamado de Polimorfismo.

Quando testamos esta classe, temos a seguinte saída:

{% highlight java %}
Autor: Cristiano
Titulo: Classes Java em fila indiana.

---------

Autor: Cristiano
Titulo: Viajando no Java

---------

Autor: Sakurai
Titulo: Almoçando com Java

---------

Autor: Sakurai
Titulo: Java em todo lugar

---------
{% endhighlight %}

2. Crie uma classe Animal que possui os atributos especie e raca, faça está classe implementar a interface Comparavel, e implemente o método comparar de forma que compare por especie e raca.

Teste a ordenação da classe Animal.