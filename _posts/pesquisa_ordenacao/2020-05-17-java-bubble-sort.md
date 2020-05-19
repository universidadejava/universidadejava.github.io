---
layout: article
title: "Java - Bubble Sort"
categories: pesquisa_ordenacao
author: sakurai
date: 2020-05-17 16:58:00
tags: [java, bubble sort, ordenação]
published: true
excerpt: O Bubble-Sort é um dos algoritmos de ordenação mais simples, que consiste em percorrer os N elementos de um vetor, para cada vez percorrida, todos os elementos são comparados com o seu próximo, para verificar se estão na ordem desejada.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Bubble Sort

O Bubble-Sort é um dos algoritmos de ordenação mais simples, que consiste em percorrer os N elementos de um vetor, para cada vez percorrida, todos os elementos são comparados com o seu próximo, para verificar se estão na ordem desejada.

Execução do algoritmo de Bubble Sort

<figure>
    <a href="/images/2020-05-17-java-bubble-sort-01.png"><img src="/images/2020-05-17-java-bubble-sort-01.png" alt="Execução do algoritmo Bubble Sort."></a>
</figure>

Na primeira iteração, é encontrado o maior elemento e o mesmo é deslocado até a ultima posição. Na segunda iteração, é encontrado o segundo maior elemento e o mesmo é deslocado até a penúltima posição. Continua até que todos os elementos serem ordenados.

> Não é obrigatório colocar sempre o maior elemento no final do vetor, dependendo da sua lógica de programação, é possível deixar os elementos de forma decrescente.

### Implementação do Bubble Sort

{% highlight java %}
package bubblesort;

public class BubbleSort {
  public static void main(String args[]) {
    int[] v = {5, 2, 4, 3, 0, 9, 7, 8, 1, 6};
    BubbleSort bs = new BubbleSort();
    bs.ordenar(v);
    for(int num : v) {
      System.out.print(num + " ");
    }
  }
  
  /**
   * Método que ordena um vetor de inteiros utilizando o algoritmo
   * de Bubble Sort.
   * 
   * @param v - Vetor que será ordenado.
   */
  public void ordenar(int[] v) {
    // for utilizado para controlar a quantidade de vezes que o vetor será ordenado.
    for(int i = 0; i < v.length - 1; i++) {
      // for utilizado para ordenar o vetor.
      for(int j = 0; j < v.length - 1 - i; j++) {
        /* Se o valor da posição atual do vetor for maior que o proximo valor,
          então troca os valores de lugar no vetor. */
        if(v[j] > v[j + 1]) {
          int aux = v[j];
          v[j] = v[j + 1];
          v[j + 1] = aux;
        }
      }
    }
  }
}
{% endhighlight %}

- Na linha 19 temos a assinatura do método que ordena um vetor de inteiros.
- Na linha 21 temos um for para controlar a quantidade de vezes que esse vetor será ordenado, no caso (v.length – 1) vezes.
- Na linha 23 temos um for para ordenar os elementos do vetor, este for irá ordenar (v.length – 1 – i) vezes. Na quantidade de vezes que o vetor é ordenado subtraímos pela quantidade de iterações que será realizada no caso a variável i, porque sabemos que quando uma iteração termina o ultimo elemento já está ordenado.
- Na linha 26 verificamos se o valor da posição atual do vetor é maior que o próximo valor do vetor, se for maior trocamos os valores de lugar.

Uma forma melhorada do Bubble Sort

Se repararmos na execução do Bubble Sort demonstrado no exemplo anterior, podemos perceber que se o vetor já estiver ordenado antes de chamar o método que ordena, será realizado as (v.length – 1) vezes iterações sobre ele e será comparado todos elementos dele para ver se está ordenado.

Podemos melhorar isso adicionando uma variável de controle que verifica se houve troca de valores no vetor, porque se durante uma iteração não houver nenhuma troca de valor, significa que o vetor já está ordenado.

Versão melhorada do Bubble Sort:

{% highlight java %}
package bubblesort;

public class BubbleSortMelhorado {
  public static void main(String args[]) {
    int[] v = {0, 1, 2, 3};
    BubbleSortMelhorado bs = new BubbleSortMelhorado();
    bs.ordenar(v);
    for(int num : v) {
      System.out.print(num + " ");
    }
  }
  
  /**
   * Método que ordena um vetor de inteiros utilizando o algoritmo
   * de Bubble Sort.
   * 
   * @param v - Vetor que será ordenado.
   */
  public void ordenar(int[] v) {
    /* for utilizado para controlar a quantidade de vezes que o vetor será
	   ordenado. */
    for(int i = 0; i < v.length - 1; i++) {
      // Variavel utilizada para controlar se o vetor ja está ordenado.
      boolean estaOrdenado = true;
      // for utilizado para ordenar o vetor.
      for(int j = 0; j < v.length - 1 - i; j++) {
        /* Se o valor da posição atual do vetor for maior que o proximo valor,
          então troca os valores de lugar no vetor. */
        if(v[j] > v[j + 1]) {
          int aux = v[j];
          v[j] = v[j + 1];
          v[j + 1] = aux;
          estaOrdenado = false;
        }
      }
      // Se o vetor está ordenado então para as iterações sobre ele.
      if(estaOrdenado)
        break;
    }
  }
}
{% endhighlight %}

- Na linha 23 foi criada uma variável boolean para controlar se o vetor está ordenado.
- Na linha 32 se houve alguma troca de valor, então o vetor ainda não está totalmente ordenado, e a variável de controle é alterada para falso.
- Na linha 36 se o vetor estiver ordenado então para a execução do for que controla a quantidade de vezes que o vetor será ordenado.

### Exemplo de ordenação de vetor de objetos

Neste exemplo temos uma classe Animal e cada animal tem uma especie e um nome.

{% highlight java %}
package bubblesort;

public class Animal {
  private String especie;
  private String nome;

  public Animal(String especie, String nome) {
    this.especie = especie;
    this.nome = nome;
  }

  public String getEspecie() {
    return especie;
  }

  public void setEspecie(String especie) {
    this.especie = especie;
  }

  public String getNome() {
    return nome;
  }

  public void setNome(String nome) {
    this.nome = nome;
  }
}
{% endhighlight %}

A partir da classe Animal, criamos um vetor de animais e queremos ordenar os animais pelo nome.

<figure>
    <a href="/images/2020-05-17-java-bubble-sort-02.png"><img src="/images/2020-05-17-java-bubble-sort-02.png" alt="Ordenando um vetor de objetos com Bubble Sort."></a>
</figure>

Segue abaixo a implementação do programa que ordena o vetor de animais utilizando o algoritmo de Bubble Sort.

{% highlight java %}
package bubblesort;

public class NomeAnimal {
  public static void main(String[] args) {
    Animal bidu = new Animal("Cachorro", "Bidu");
    Animal fred = new Animal("Peixe", "Fred");
    Animal rex = new Animal("Cachorro", "Rex");
    Animal akamaru = new Animal("Cachorro", "Akamaru");
    Animal mingau = new Animal("Gato", "Mingau");
    
    Animal[] animais = new Animal[] {bidu, fred, rex, akamaru, mingau};
    
    NomeAnimal na = new NomeAnimal();
    na.ordenarAnimaisPorNome(animais);
    
    for(int tamanho = 0; tamanho < animais.length; tamanho++) {
      System.out.println("Especie: " + animais[tamanho].getEspecie() +
        " - Nome: " + animais[tamanho].getNome());
    }
  }
  
  /**
   * Método que ordena um vetor de Animal utilizando o algortimo Bubble Sort,
   * a ordenação é feita de acordo com o nome de cada animal.
   * 
   * @param animais - Vetor de Animal.
   */
  public void ordenarAnimaisPorNome(Animal[] animais) {
    /* for utilizado para controlar a quantidade de vezes que o vetor será
	   ordenado. */
    for(int i = 0; i < animais.length - 1; i++) {
      // Variavel utilizada para controlar se o vetor ja está ordenado.
      boolean estaOrdenado = true;
      // for utilizado para ordenar o vetor.
      for(int j = 0; j < animais.length - 1 - i; j++) {
        /* Se o nome do animal na posição atual do vetor for maior que o nome
           do proximo animal, então troca os Animais de lugar no vetor. */
        if(animais[j].getNome().compareToIgnoreCase(animais[j + 1].getNome()) > 0) {
          Animal aux = animais[j];
          animais[j] = animais[j + 1];
          animais[j + 1] = aux;
          estaOrdenado = false;
        }
      }
      // Se o vetor está ordenado então para as iterações sobre ele.
      if(estaOrdenado)
        break;
    }
  }
}
{% endhighlight %}

- Na linha 28 temos a assinatura do método que ordena um vetor de animais.
- Na linha 30 temos um for para controlar a quantidade de vezes que esse vetor será ordenado, no caso (animais.length – 1) vezes.
- Na linha 32 foi criada uma variável boolean para controlar se o vetor está ordenado.
- Na linha 34 temos um for para ordenar os elementos do vetor, este for irá ordenar (animais.length – 1 – i) vezes. Na quantidade de vezes que o vetor é ordenado subtraímos pela quantidade de iterações que será realizada no caso a variável i, porque sabemos que quando uma iteração termina o ultimo elemento já está ordenado.
- Na linha 37 verificamos se nome do animal da posição atual do vetor é maior que o nome do próximo animal do vetor, se for maior trocamos os animais de lugar.
- Na linha 41 se houve alguma troca de animal, então o vetor ainda não está totalmente ordenado, e a variável de controle é alterada para falso.
- Na linha 45 se o vetor estiver ordenado então para a execução do for que controla a quantidade de vezes que o vetor será ordenado.

Também podemos ordenar os animais pela especie e depois pelo nome do animal:

{% highlight java %}
package bubblesort;

public class NomeAnimal {
  public static void main(String[] args) {
    Animal bidu = new Animal("Cachorro", "Bidu");
    Animal fred = new Animal("Peixe", "Fred");
    Animal rex = new Animal("Cachorro", "Rex");
    Animal akamaru = new Animal("Cachorro", "Akamaru");
    Animal mingau = new Animal("Gato", "Mingau");
    
    Animal[] animais = new Animal[] {bidu, fred, rex, akamaru, mingau};
    
    NomeAnimal na = new NomeAnimal();
    na.ordenarAnimaisPorRacaENome(animais);
    
    for(int tamanho = 0; tamanho < animais.length; tamanho++) {
      System.out.println("Especie: " + animais[tamanho].getEspecie() +
        " - Nome: " + animais[tamanho].getNome());
    }
  }
  
  /**
   * Método que ordena um vetor de Animal utilizando o algortimo Bubble Sort,
   * a ordenação é feita de acordo com a especie e nome de cada animal.
   * 
   * @param animais - Vetor de Animal.
   */
  public void ordenarAnimaisPorRacaENome(Animal[] animais) {
    /* for utilizado para controlar a quantidade de vezes que o vetor será
	   ordenado. */
    for(int i = 0; i < animais.length - 1; i++) {
      // Variavel utilizada para controlar se o vetor ja está ordenado.
      boolean estaOrdenado = true;
      // for utilizado para ordenar o vetor.
      for(int j = 0; j < animais.length - 1 - i; j++) {
        /* Se o nome da especie do animal na posição atual do vetor for maior 
           que o nome da especie do proximo animal, então troca os animais
		   de lugar no vetor. */
        if(animais[j].getEspecie().compareToIgnoreCase(animais[j + 1].getEspecie()) > 0) {
          Animal aux = animais[j];
          animais[j] = animais[j + 1];
          animais[j + 1] = aux;
          estaOrdenado = false;
        }
        /* Se o nome do da especie do animal na posição atual do vetor for
           igual o nome da especie do proximo animal, e o nome do animal
           na posição atual do vetor for maior que o nome do proximo animel,
		   então troca os animais de lugar no vetor. */
        else if(animais[j].getEspecie().equals(animais[j + 1].getEspecie()) &&
          animais[j].getNome().compareToIgnoreCase(animais[j + 1].getNome()) > 0) {
          Animal aux = animais[j];
          animais[j] = animais[j + 1];
          animais[j + 1] = aux;
        }
      }
      // Se o vetor está ordenado então para as iterações sobre ele.
      if(estaOrdenado)
        break;
    }
  }
}
{% endhighlight %}

- Na linha 65 verifica se o nome da especie do animal na posição atual do vetor é maior que a especie do próximo animal, se for então troca os animais de lugar.
- Na linha 74 verifica se o nome da especie do animal na posição atual do vetor é igual a especie do próximo animal, se as espécies forem iguais, então verifica se o nome do animal na posição atual do vetor é maior que o nome do próximo animal, se for maior então troca os animais de lugar no vetor.