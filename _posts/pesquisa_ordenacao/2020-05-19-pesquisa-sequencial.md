---
layout: article
title: "Pesquisa de Dados - Sequencial"
categories: pesquisa_ordenacao
author: sakurai
date: 2020-05-19 22:40:00
tags: [java, pesquisa, sequencial]
published: true
excerpt: O algoritmo de pesquisa sequencial possui este nome porque o objetivo dele é procurar o elemento de forma sequencialmente dentro de uma coleção de elementos.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Pesquisa Sequencial

O objetivo da pesquisa é procurar por um elemento dentro de uma coleção de elementos. O algoritmo de pesquisa sequencial possui este nome porque o objetivo dele é procurar o elemento de forma sequencialmente dentro de uma coleção de elementos.

Dado um vetor:

{% highlight java %}
[5, 4, 0, 8, 7, 9]
{% endhighlight %}

- Percorre os elementos de um vetor
- Se o elemento procurado estiver dentro do vetor, imprime o valor do elemento e para de procurar pelo elemento no vetor.
- Se não encontrou o elemento dentro do vetor, imprime uma mensagem informando que o elemento não foi encontrado.


### Pesquisando o valor 0 (zero)

Quando pesquisamos o número 0 (zero) dentro do vetor temos o seguinte fluxo:

O contador está com valor 0 (zero) e verifica se o número dentro do vetor na posição do contador é igual ao número procurado 0 (zero).

<figure>
    <a href="/images/2020-05-19-pesquisa-sequencial-01.png"><img src="/images/2020-05-19-pesquisa-sequencial-01.png" alt="O contador está com valor 0 (zero) e verifica se o número dentro do vetor na posição do contador é igual ao número procurado 0 (zero)."></a>
</figure>

Como o número que está dentro do vetor não é o número que está sendo procurado, então incrementa em 1 o valor do contador e verifica novamente se o valor que está dentro do vetor na posição do contador é igual ao valor procurado.

<figure>
    <a href="/images/2020-05-19-pesquisa-sequencial-01.png"><img src="/images/2020-05-19-pesquisa-sequencial-01.png" alt="Como o número que está dentro do vetor não é o número que está sendo procurado, então incrementa em 1 o valor do contador e verifica novamente se o valor que está dentro do vetor na posição do contador é igual ao valor procurado."></a>
</figure>

Como o número que está dentro do vetor ainda não é o número que está sendo procurado, então incrementa em 1 o valor do contador e verifica novamente se o valor que está dentro do vetor na posição do contador é igual ao valor procurado.

<figure>
    <a href="/images/2020-05-19-pesquisa-sequencial-03.png"><img src="/images/2020-05-19-pesquisa-sequencial-03.png" alt="Como o número que está dentro do vetor ainda não é o número que está sendo procurado, então incrementa em 1 o valor do contador e verifica novamente se o valor que está dentro do vetor na posição do contador é igual ao valor procurado."></a>
</figure>
	
Encontrou o valor procurado dentro do vetor, então imprime uma mensagem avisando que encontrou o valor procurado.


### Pesquisando o valor 3 (três)

Agora queremos pesquisar o valor 3 dentro do vetor, percorremos o vetor por completo e não encontramos o valor pesquisado:

<figure>
    <a href="/images/2020-05-19-pesquisa-sequencial-04.png"><img src="/images/2020-05-19-pesquisa-sequencial-04.png" alt="Agora queremos pesquisar o valor 3 dentro do vetor."></a>
</figure>

Neste ponto o valor do contador é igual a o tamanho do vetor, ou seja, tamanho 6. Então devemos imprimir uma mensagem avisando que o valor pesquisado não está dentro do vetor.


### Implementação do algoritmo de Pesquisa Sequencial

{% highlight java %}
package pesquisa.sequencial;

public class Sequencial {
  public static void main(String[] args) {
    Sequencial seq = new Sequencial();
    
    int[] numeros = {5, 4, 0, 8, 7, 9};
    
    seq.pesquisarNumero(0, numeros);
    seq.pesquisarNumero(3, numeros);
  }
  
  /**
   * Método utilizado para procurar se um elemento está dentro do vetor.
   * Este método utiliza o algoritmo de Pesquisa Sequencial para encontrar
   * o elemento.
   * 
   * @param x    - Número que será procurado. 
   * @param numeros - Vetor de números.
   */
  public void pesquisarNumero(int x, int[] numeros) {
    int cont = 0;
    
    for(cont = 0; cont < numeros.length; cont++) {
      //Verifica se o elemento que está sendo procurado está no vetor.
      if (numeros[cont] == x) {
        //Se encontrou o elemento, imprime ele na tela e para a pesquisa.
        System.out.println("Encontrou o número " + x);
        break;
      }
    }
    
    //Verifica se não encontrou o elemento no vetor.
    if(cont >= numeros.length)
      System.out.println("Não encontrou o número " + x);
  }
}
{% endhighlight %}

- Na linha 20, temos a assinatura do método que vai procurar um elemento dentro do vetor, utilizando o algoritmo de **Pesquisa Sequencial**.
- Na linha 23, utilizamos um `for` para percorrer todos os elementos do vetor.
- Na linha 25, verifica se o numero dentro do vetor, tem o mesmo valor que o número que está sendo procurado, e se encontrou o número então imprime ele na tela e para de pesquisar.
- Na linha 33, verificamos se o tamanho do contador utilizado no for tem um valor maior que o tamanho do vetor, para verificar se já percorreu todo o vetor, mas não encontrou o valor procurado.


### Utilizando pesquisa sequencial com objetos

O mesmo conceito de pesquisa sequencial pode ser aplicado para um vetor de Objetos, procurando algum objeto por um ou vários atributos.

{% highlight java %}
package pesquisa.sequencial;

public class Sequencial2 {
  public static void main(String[] args) {
    Sequencial2 seq = new Sequencial2();
    
    Pessoa rafael = new Pessoa("Rafael", 24);
    Pessoa ana = new Pessoa("Ana", 18);
    Pessoa cristiano = new Pessoa("Cristiano", 25);
    Pessoa carla = new Pessoa("Carla", 28);
    
    Pessoa[] pessoas = {rafael, ana, cristiano, carla};
    
    Pessoa pessoaX = new Pessoa("Ana", 18);
    
    seq.pesquisarPessoa(pessoaX, pessoas);
  }
  
  /**
   * Método utilizado para procurar se um objeto Pessoa está dentro do vetor de
   * Pessoas, a procura é feita através do nome da Pessoa.
   * Utiliza o algoritmo de Pesquisa Sequencial para encontrar o elemento.
   * 
   * @param x    - Pessoa que será procurada. 
   * @param pessoas - Vetor de pessoas.
   */
  public void pesquisarPessoa(Pessoa x, Pessoa[] pessoas) {
    int cont = 0;
    
    for(cont = 0; cont < pessoas.length; cont++) {
      //Verifica se o elemento que está sendo procurado está no vetor.
      if(pessoas[cont].getNome().equals(x.getNome())) {
        //Se encontrou o elemento, imprime ele na tela e para a pesquisa.
        System.out.println("Encontrou a pessoa " + x.getNome());
        break;
      }
    }
    
    //Verifica se não encontrou o elemento no vetor.
    if(cont >= pessoas.length)
      System.out.println("Não encontrou a pessoa " + x.getNome());
  }
}
{% endhighlight %}

- Na linha 27, temos a assinatura do método que vai procurar uma Pessoa dentro do vetor de pessoas, utilizando o algoritmo de **Pesquisa Sequencial**.
- Na linha 30, utilizamos um for para percorrer todas as pessoas do vetor.
- Na linha 32, verifica se o nome da pessoa dentro do vetor, tem o mesmo nome  da pessoa que está sendo procurada, e se encontrou então imprime o nome da pessoa na tela e para de pesquisar.
- Na linha 40, verificamos se o tamanho do contador utilizado no for tem um valor maior que o tamanho do vetor, para verificar se já percorreu todo o vetor, mas não encontrou alguma pessoa com o mesmo nome.


### Outra forma de programar a pesquisa sequencial

Este mesmo algoritmo de busca sequencial pode ser programando de uma outra forma, sem afetar a estrutura do algoritmo, exemplo:

{% highlight java %}
  /**
   * Método utilizado para procurar se um elemento está dentro do vetor.
   * Este método utiliza o algoritmo de Pesquisa Sequencial para encontrar o elemento.
   * 
   * @param x    - Número que será procurado. 
   * @param numeros - Vetor de números.
   */
  public void pesquisarNumero(int x, int[] numeros) {
    int cont = 0;
    
    /* Percorre o vetor até chegar no fim ou até encontrar o número no vetor. */
    while (cont < numeros.length && numeros[cont] != x)
      cont++;
    
    /* Verifica se não percorreu todos os elementos do vetor e se encontrou o 
    número dentro do vetor. */
    if (cont < numeros.length && numeros[cont] == x)
      System.out.println("Encontrou o número " + x);
    else /* Caso tenha percorrido todo o vetor e não encontrou o número. */
      System.out.println("Não encontrou o número " + x);
  }
{% endhighlight %}

- Na linha 24, criamos um laço que enquanto não chegar ao fim do vetor ou não encontrar o valor procurado dentro do vetor, vai incrementando a variável cont.
- Na linha 29, verifica se ainda não percorreu todos os elementos do vetor e se encontrou o valor procurado.
- A linha 32 é executada se chegou ao fim do vetor e não encontrou o valor procurado.


### Conteúdos relacionados

- [Pesquisa binária](http://www.universidadejava.com.br/pesquisa_ordenacao/pesquisa-binaria/)
- [Ordenação de dados com Quick Sort](http://www.universidadejava.com.br/pesquisa_ordenacao/quick-sort/)