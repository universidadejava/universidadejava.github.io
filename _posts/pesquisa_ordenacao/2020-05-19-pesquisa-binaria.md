---
layout: article
title: "Pesquisa de Dados - Binária"
categories: pesquisa_ordenacao
author: sakurai
date: 2020-05-19 23:00:00
tags: [java, pesquisa, binaria]
published: true
excerpt: Este método de pesquisa é muito mais rápido que a pesquisa sequencial, e usa como base que o vetor já está ordenado.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Pesquisa Binária

Este método de pesquisa é muito mais rápido que a pesquisa sequencial, e usa como base que o vetor já está ordenado:

Dado um vetor:

{% highlight java %}
[0, 1, 2, 4, 5, 6, 7, 8, 9]
{% endhighlight %}

Encontra a posição referente ao meio do vetor, e verifica se o valor procurado é o elemento do meio do vetor.
Se encontrou o valor:
- imprime uma mensagem de confirmação.
Se não encontrou o valor:
- Se o valor procurado é menor que o valor que está no meio do vetor, a posição final do vetor será uma posição antes do meio do vetor.
- Se o valor procurado é maior que o valor que está no meio do vetor, a posição inicial do vetor será uma posição depois do meio do vetor.
- Volta para o passo 1.


### Procurando o valor 4 dentro do vetor

- O valor da posição inicial é 0 (zero).
- O valor da posição final é 8.
- O valor da posição meio é 4. (A posição do meio é igual ao `(inicio + fim) / 2`)

Verifica se o valor do vetor que está na posição do meio é igual ao valor procurado, neste caso o valor do meio do vetor é 5 e estamos procurando o valor 4.

<figure>
    <a href="/images/2020-05-19-pesquisa-binaria-01.png"><img src="/images/2020-05-19-pesquisa-binaria-01.png" alt="Verifica se o valor do vetor que está na posição do meio é igual ao valor procurado."></a>
</figure>

Como o valor que estamos procurando é menor que o valor que está no meio do vetor, então mudamos o valor da posição final para 3. O valor da posição inicial continua sendo 0 (zero). Agora o valor da posição meio muda 1.

Verifica novamente se o valor do vetor na posição do meio é igual ao valor procurado, neste caso o valor do meio do vetor é 1 e estamos procurando o valor 4.

<figure>
    <a href="/images/2020-05-19-pesquisa-binaria-02.png"><img src="/images/2020-05-19-pesquisa-binaria-02.png" alt="Verifica novamente se o valor do vetor na posição do meio é igual ao valor procurado."></a>
</figure>

Como o valor que estamos procurando é maior que o valor que está no meio do vetor, então mudamos o valor da posição inicial para 2. O valor da posição final continua sendo 3. Agora o valor da posição meio muda 2.

Verifica novamente se o valor do vetor na posição do meio é igual ao valor procurado, neste caso o valor do meio do vetor é 2 e estamos procurando o valor 4.

<figure>
    <a href="/images/2020-05-19-pesquisa-binaria-03.png"><img src="/images/2020-05-19-pesquisa-binaria-03.png" alt="Verifica novamente se o valor do vetor na posição do meio é igual ao valor procurado."></a>
</figure>

Como o valor que estamos procurando é maior que o valor que está no meio do vetor, então mudamos o valor da posição inicial para 3. O valor da posição final continua sendo 3. Agora o valor da posição meio muda 3.

Verifica novamente se o valor do vetor na posição do meio é igual ao valor procurado, neste caso o valor do meio do vetor é 4 e encontramos o valor que estávamos procurando.

<figure>
    <a href="/images/2020-05-19-pesquisa-binaria-04.png"><img src="/images/2020-05-19-pesquisa-binaria-04.png" alt="Verifica novamente se o valor do vetor na posição do meio é igual ao valor procurado."></a>
</figure>


### Implementação do algoritmo de Pesquisa Binária

{% highlight java %}
package pesquisa.binaria;

/**
 * Classe utilizada para demonstrar o algoritmo Pesquisa Binaria.
 */
public class PesquisaBinaria {
  public static void main(String[] args) {
    PesquisaBinaria bin = new PesquisaBinaria();
    
    int[] numeros = {1, 3, 4, 7, 9, 10, 13, 18, 20, 21, 22};
    
    bin.pesquisarNumero(20, numeros);
    bin.pesquisarNumero(3, numeros);
  }
  
  /**
   * Método que pesquisa um número dentro de um vetor de números.
   * Este método utiliza o algoritmo de pesquisa binaria.
   * 
   * @param x    - Número que será pesquisado.
   * @param numeros - Vetor de números.
   */
  public void pesquisarNumero(int x, int[] numeros) {
    int inicio = 0;         //Posição inicial do vetor.
    int meio = 0;          //Posição do meio do vetor.
    int fim = numeros.length - 1;  //Posição final do vetor.
    
    /* Enquanto a posição do inicio for menor ou igual a posição do fim,
      procura o valor de x dentro do vetor. */
    while(inicio <= fim) {
      meio = (fim + inicio) / 2; //Encontra o meio do vetor.
      
      System.out.println("Inicio: " + numeros[inicio] + " - Meio: " + numeros[meio] + " - Fim: " + numeros[fim]);
      System.out.println("Inicio: " + inicio + " - Meio: " + meio + " - Fim: " + fim);
      
      /* Se o valor que está no meio do vetor é igual ao valor procurado, 
        imprime que encontrou o valor e para de pesquisar. */
      if(numeros[meio] == x) {
        System.out.println("Encontrou o número " + x);
        break;
      }
      
      /* Este if serve para diminuir o tamanho do vetor pela métade. */
      /* Se o valor que está no meio do vetor for menor que o valor de x, 
        então o inicio do vetor será igual a posição do meio + 1. */
      if(numeros[meio] < x) {
        inicio = meio + 1;
      } else {
        /* Se o valor que está no meio do vetor for maior que o valor de x, 
          então o fim do vetor será igual a posição do meio - 1. */
        fim = meio - 1;
      }
    }
    
    /* Caso não encontre o valor pesquisado dentro do vetor,
      imprime que não encontrou o valor pesquisado. */
    if(inicio > fim) {
      System.out.println("Não encontrou o número " + x);
    }
  }
}
{% endhighlight %}

- Na linha 20, declaramos a assinatura do método que vai procurar por um número dentro de um vetor de números.
- Na linha 27, criamos um laço que será executado até encontrarmos o número procurado ou até a posição inicial ficar maior que a posição final (não encontrar o número dentro do vetor).
- Na linha 28, calculamos a posição meio do vetor.
- Na linha 32, verifica se o valor que tem no vetor na posição meio é igual ao valor do número procurado, se for igual, então imprimimos seu valor e paramos a pesquisa.
- Na linha 40, se não encontramos o valor, vamos diminuir o tamanho do vetor, para restringir o local onde pode estar o valor procurado.
- Se o valor do meio do vetor for menor que o valor procurado, significa que o valor que estamos procurando está entre a posição do `meio + 1` e o `fim` do vetor.
- Se o valor do meio do vetor for maior que o valor procurado, significa que o valor que estamos procurando está entre a posição `inicial` e o `meio – 1` do vetor.
- A linha 52 é executada apenas se não encontrou o número procurado dentro do vetor.


### Pesquisa Binária utilizando objetos

Dado um vetor de pessoas, queremos pesquisar as pessoas que tem a inicial do nome entre 'A' e 'F'.

> Neste exemplo não vamos tratar mais de um nome com a mesma inicial e também não vamos tratar intervalos de letras que não tem nome, exemplo: 'G' a 'H'.

{% highlight java %}
package pesquisa.binaria;

/**
 * Exemplo de uso do algoritmo Pesquisa Binaria, para pesquisar nomes
 * dentro de um intervalo de caracteres iniciais.
 */
public class PesquisarNomes {
  public static void main(String[] args) {
    PesquisarNomes pesquisarNomes = new PesquisarNomes();
    
    Pessoa ana = new Pessoa("Ana", 18);
    Pessoa carla = new Pessoa("Carla", 20);
    Pessoa felipe = new Pessoa("Felipe", 24);
    Pessoa patricia = new Pessoa("Patricia", 23);
    Pessoa rafael = new Pessoa("Rafael", 20);
    
    Pessoa[] pessoas = {ana, carla, felipe, patricia, rafael};
    
    int inicio = pesquisarNomes.pesquisar('A', pessoas);
    int fim = pesquisarNomes.pesquisar('F', pessoas);

    /* Imprime os nomes encontrados. */
    while(inicio >= 0 && inicio <= fim) {
      System.out.println(pessoas[inicio].getNome());
      inicio++;
    }
  }
}
{% endhighlight %}

Neste exemplo criamos algumas pessoas e colocamos elas dentro de um vetor, depois procuramos qual a posição do vetor está a pessoa com a letra 'A' e também procuramos a posição do vetor está a pessoa com a letra 'F'. Depois imprimimos todas as pessoas que estão neste intervalo.

{% highlight java %}
  /**
   * Método utilizado para pesquisar uma pessoa que tem a primeira 
   * letra do nome igual a letra recebida.
   * Este método utiliza o algoritmo de pesquisa binaria.
   * 
   * @param letra  - Letra inicial do nome.
   * @param pessoas - Vetor de pessoas.
   * @return posição em que se encontra a pessoa com a letra pesquisada. Caso não
   * tenha nenhuma pessoa dentro do vetor com a letra igual a pesquisada, então 
   * retorna a posição mais proxima.
   */
  public int pesquisar(char letra, Pessoa[] pessoas) {
    int inicio = 0;         //Posição inicial do vetor.
    int meio = 0;          //Posição do meio do vetor.
    int fim = pessoas.length - 1;  //Posição final do vetor.
    
    /* Enquanto a posição do inicio for menor ou igual a posição do fim,
      procura o valor de x dentro do vetor. */
    while(inicio <= fim) {
      meio = (fim + inicio) / 2; //Encontra o meio do vetor.
      
      /* Se a primeira letra do nome da pessoa que está no meio do vetor for 
        igual a letra procurada, retorna o valor da posição do meio do vetor 
        e para de pesquisar. */
      if(pessoas[meio].getNome().charAt(0) == letra) {
        return meio;
      }
      
      /* Este if serve para diminuir o tamanho do vetor pela métade. */
      /* Se a primeira letra do nome da pessoa que está no meio do vetor for
        menor que o valor da letra procurada, então o inicio do vetor seráa 
        igual a posição do meio + 1. */
      if(pessoas[meio].getNome().charAt(0) < letra) {
        inicio = meio + 1;
      } else {
        /* Se a primeira letra do nome da pessoa que está no meio do vetor
          for maior que o valor da letra procurada, então o fim do vetor 
          será igual a posição do meio - 1. */
        fim = meio - 1;
      }
    }

    /* Se não encontrou nenhuma pessoa que tenha a primeira letra do nome 
      igual a letra que está sendo procurada, então retorna a posição do
      vetor que possui a letra mais proxima. */
    return fim;
  }
{% endhighlight %}

- Na linha 37, declaramos a assinatura do método que recebe uma letra e um vetor de pessoas.
- Na linha 50 verificamos se a primeira letra do nome da pessoa que está no meio do vetor é igual a letra procurada. Se encontrou o nome, então retorna a posição do vetor.
- Na linha 58, se não encontramos o nome com a letra inicial igual a letra procurada, vamos diminuir o tamanho do vetor, para restringir o local onde possa estar.
- Se a letra do nome do meio do vetor for menor que a letra procurada, significa que o nome com a letra que estamos procurando está entre a posição do `meio + 1` e o fim do vetor.
- Se a letra do nome do meio do vetor for maior que a letra procurada, significa que o nome com a letra que estamos procurando está entre a posição inicial e o `meio – 1` do vetor.
- A linha 57 é executada apenas se não encontrou o número procurado dentro do vetor, então retorna a posição que possui um caractere mais próximo do procurado.


### Conteúdos relacionados

- [Pesquisa sequencial](http://www.universidadejava.com.br/pesquisa_ordenacao/pesquisa-sequencial/)
- [Ordenação de dados com Merge Sort](http://www.universidadejava.com.br/pesquisa_ordenacao/merge-sort/)
- [Ordenação de dados com Quick Sort](http://www.universidadejava.com.br/pesquisa_ordenacao/quick-sort/)