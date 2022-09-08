---
layout: article
title: "Fibonacci implementação recursiva"
categories: java
author: sakurai
date: 2020-05-16 23:27:00
tags: [java, recursividade, fibonacci]
published: true
excerpt: Exemplo de método recursivo que calcular Fibonacci.
comments: true
image:
  teaser: 2020-05-16-teaser-java-fibonacci.png
ads: false
---

## Exemplo fibonacci

Método recursivo que calcula o valor de fibonacci para um determinado valor `x`.

Sequência de fibonacci:

{% highlight java %}
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233
{% endhighlight %}

Definição recursiva pela forma matemática:

<figure>
    <a href="/images/2020-05-16-java-fibonacci-01.png"><img src="/images/2020-05-16-java-fibonacci-01.png" alt="Definição matemática de fibonacci."></a>
</figure>

Implementação do método que calcula o valor de fibonacci.

{% highlight java %}
package material.recursao;

public class Fibonacci {
  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println(fib.fibonacci(10));
  }
    
  /**
   * Método que calcula o valor de fibonacci para um determinado valor x.
   * 
   * Sequência de fibonacci
   *  0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, ...
   * 
   * @param x - Valor que será calculado.
   * @return - O valor de fibonacci para o valor x.
   */
  public int fibonacci(int x) {
    /* Se x for igual a 0 (zero) ou 1, então o valor de fibonacci é
       o proprio valor x. */
    if (x == 0 || x == 1 )
      return x;

    /* Se o valor de x for maior que 1, então precisa calcular o valor
       do fibonacci. */
    return fibonacci(x - 1) + fibonacci(x - 2);
  }
}
{% endhighlight %}

No fibonacci sabemos que o momento de parada do método é quando o `x` vale `0` (zero) ou `1`, neste caso sua resposta é conhecida.

{% highlight java %}
if (x == 0 || x == 1 )
  return x;
{% endhighlight %}

Para qualquer outro valor de `x` diferente de `0` (zero) e `1` não sabemos sua resposta, portanto precisamos calcular o valor do fibonacci.

{% highlight java %}
return fibonacci(x - 1) + fibonacci(x - 2);
{% endhighlight %}

Ou seja, para calcular o fibonacci de `2`, preciso primeiro calcular seu valor para `1` e para `0 (zero) e somar os dois resultados

Nesta figura temos a sequência de métodos `public int fibonacci(int x)` que serão chamados, também mostra qual o valor `x` será passado para o método e qual o valor retornado pelo método:

<figure>
    <a href="/images/2020-05-16-java-fibonacci-02.png"><img src="/images/2020-05-16-java-fibonacci-02.png" alt="Chamada recursiva do calculo do fibonacci."></a>
</figure>

Os itens em verde são as sequências em que o método `fibonacci(int x)` será executado. Os itens em vermelho são os resultados do fibonacci para o valor `x` recebido.


### Conteúdos relacionados

- [Implementando métodos recursivos no Java](http://www.universidadejava.com.br/java/java-recursividade/)
- [Calculando o fatorial de um número de forma recursiva no Java](http://www.universidadejava.com.br/java/java-fatorial/)
- [Implementando uma busca binária no Java](http://www.universidadejava.com.br/pesquisa_ordenacao/pesquisa-binaria/)
- [Entendendo como funciona a implementaçãdo do Quick Sort](http://www.universidadejava.com.br/pesquisa_ordenacao/quick-sort/)